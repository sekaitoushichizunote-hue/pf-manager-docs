# データ連携・初期設定ガイド

PF_Manager は、複数の証券会社・ブローカーから取得したデータを統合し、ポートフォリオを一元管理する投資家向けアプリケーションです。本書では、ユーザーが最もつまずきやすいデータソース連携手順を詳しく解説します。

---

## 重要なセキュリティ注意事項

!!! danger "セキュリティに関するご注意"
    - **APIキー、Token、App Secret、パスワード、Trading PIN は絶対に第三者へ共有しないでください。**
    - 設定ファイルを GitHub 等の公開リポジトリへアップロードしないでください。
    - 不審な連絡で認証情報の提供を求められても絶対に応じないでください。
    - 定期的なトークン再発行を推奨します。

!!! info "安心・安全のための補足"
    本アプリ（PF_Manager）内で Webull 等の連携のために入力いただくパスワードや PIN 情報は、お客様ご自身の PC 内（ローカル環境）にのみ保存されます。外部の第三者サーバーへ送信されることは一切ありませんので、ご安心ください。

---

## 対応証券会社とインポートデータ一覧

PF_Manager が対応している証券会社と、取得可能なデータ項目、および連携方式は以下の通りです。ご自身の利用している証券会社の「インポート形式」を確認し、該当する手順の項目にお進みください。

| 証券会社 | 区分 | 連携方式 |
|---|---|---|
| 楽天証券 | 国内 | CSV インポート |
| SBI証券 | 国内 | CSV インポート |
| マネックス証券 | 国内 | CSV インポート |
| moomoo証券 | 国内 | CSV インポート |
| 松井証券 / GMOクリック証券 / 三菱UFJ eスマート証券 など | 国内 | CSV インポート |
| Interactive Brokers (IBKR) | 海外 | Flex Query (XML) |
| Saxo Bank | 海外 | Open API |
| Webull | 海外 | CSV インポート |

---

## 国内証券会社のデータインポート

PF_Manager は、主要な国内証券会社の CSV フォーマットを自動で判別し、インポートする機能を備えています。特別の指定がない限り、証券会社の PC 版 Web サイトにログインし各画面から CSV ファイルをダウンロードしてください。

!!! note "共通の注意事項"
    ダウンロードした CSV ファイルは、**ファイル名や中身を一切変更せず**に `download` フォルダに保管するか、PF_Manager の `Data\inputs` へ保管してください。システムがファイル名やヘッダーから証券会社を自動判別します。

---

### 1. 楽天証券

=== "保有銘柄（ポジション）"

    <span class="pf-step">1</span> ログイン後、上部メニューの「**マイメニュー**」>「**資産残高・保有商品**」を開きます。

    <span class="pf-step">2</span> 国内株式、米国株式、投資信託などの各タブを開き、明細の右上付近にある「**CSVダウンロード**」をクリックします。

=== "取引履歴・配当金"

    <span class="pf-step">1</span> 「**マイメニュー**」>「**取引履歴**」または「**配当・分配金**」画面を開きます。

    <span class="pf-step">2</span> 取得したい期間を指定して「**CSV**」ボタンをクリックします。

---

### 2. SBI証券

=== "保有銘柄（ポジション）"

    <span class="pf-step">1</span> ログイン後、「**口座管理**」>「**口座(円建)**」または「**口座(外貨建)**」>「**保有証券・資産**」タブを開きます。

    <span class="pf-step">2</span> 画面内にある「**CSVダウンロード**」をクリックします。

=== "取引履歴・配当金"

    <span class="pf-step">1</span> 「**口座管理**」>「**取引履歴**」や「**入出金・振替**」画面を開きます。

    <span class="pf-step">2</span> 該当の履歴を表示して「**CSVダウンロード**」をクリックします。

---

### 3. マネックス証券

=== "保有銘柄（ポジション）"

    <span class="pf-step">1</span> ログイン後、「**保有残高・口座管理**」を開きます。

    <span class="pf-step">2</span> 資産明細の右上にある「**CSV形式で保存**」をクリックします。

=== "取引履歴・配当金"

    <span class="pf-step">1</span> 「**取引履歴・損益**」画面を開きます。

    <span class="pf-step">2</span> 期間を指定し、CSV をダウンロードします。

---

### 4. moomoo証券

PC 版アプリから、「**口座**」>「**履歴**」や「**建玉（ポジション）**」画面を開き、右上にあるエクスポート（ダウンロード）アイコンから CSV 形式で出力します。

---

### 5. その他の対応証券会社

松井証券、GMOクリック証券、三菱UFJ eスマート証券 など

基本的にはどの証券会社も、「**資産残高（保有商品）**」や「**取引履歴（約定履歴）**」のページ内に、「CSV ダウンロード」や「エクスポート」というボタンが用意されています。それらをクリックしてファイルを取得してください。

---

## 海外証券会社のデータインポート

### 1. Interactive Brokers (IBKR)

PF_Manager では『**Activity Flex Query**』を利用し、XML 形式でデータ取得を行います。

ここでは、Flex Query の作成と、Flex Web Service を有効化し、Query ID / Token ID を取得する手順を説明します。

#### Flex Query の作成

<span class="pf-step">1</span> クライアントポータル（Web）へログイン → トップページ → **Performance & Reports** → **Flex Queries** を開きます。

<span class="pf-step">2</span> Flex Queries 画面に移動したことを確認し、**＋ボタン**をクリックします。

<span class="pf-step">3</span> Flex Query の作成画面が表示されます。上から順に次のように入力・選択してください。

| 項目 | 設定値 |
|---|---|
| **Query Name** | 任意の名称を入力 |
| **Sections** | 下記の全てを選択 |
| **Format** | `XML` を選択 |
| **Period** | 任意で指定可能（期間が長いとダウンロードに時間がかかります） |

**Sections で選択する項目（全て選択してください）**

- Open Positions
- Trades
- Cash Report
- Interest Accruals
- Cash Transactions
- Realized and Unrealized Performance Summary in Base

<span class="pf-step">4</span> その他の項目はデフォルトのまま「**Continue**」をクリックし、設定を **Save** します。

<span class="pf-step">5</span> 保存後、作成された Flex Query の横にある**鉛筆マーク**をクリックします。詳細画面が表示されるので、**Query ID** を控えます。

#### Flex Web Service の有効化

<span class="pf-step">1</span> Flex Queries 画面へ戻り、右下の「**Flex Web Service Configuration**」エリアの**歯車マーク**をクリックします。

<span class="pf-step">2</span> **Flex Web Service Status** の横にあるチェックボックスをオンにし、**Save** をクリックします。

<span class="pf-step">3</span> 再度、歯車マークから Config 画面へ進むと「**Current Token**」が表示されるので、この値を控えます。

<span class="pf-step">4</span> PF Manager の「**Settings**」>「**Interactive Brokers（Flex Query）**」の部分へ、控えた **Token ID** と **Query ID** を入力し、画面最下部の「**Save All Settings**」をクリックして保存します。

---

### 2. Saxo Bank

PF_Manager では『**Open API**』を利用し、データ取得を行います。

ここでは、Saxo Bank Developer ID を取得し、Live App を作成。PF Manager へ App Key 等を入力し、初回の API インポート動作時の認証トークン作成までを説明します。

#### Saxo Bank Developer ID の取得

<span class="pf-step">1</span> `https://www.developer.saxo/accounts/sim/signup` へアクセスします。

<span class="pf-step">2</span> アカウント作成画面が表示されるので、必要事項を入力します。

<span class="pf-step">3</span> アカウント作成後は、メールで **User ID** と **Password** が送付されます。

#### Live Apps の作成

<span class="pf-step">1</span> `https://www.developer.saxo/` へアクセスします。

<span class="pf-step">2</span> 画面右上の「ハンバーガーメニュー」をクリックし、「**Sign In**」をクリックします。

<span class="pf-step">3</span> 先ほど登録した Saxo Bank Developers ID/Password でログインします。

<span class="pf-step">4</span> 右上の「ハンバーガーメニュー」>「**Apps**」をクリックします。

<span class="pf-step">5</span> Application Management 画面で、左サイドメニューの「**Live Apps**」をクリックします。

<span class="pf-step">6</span> Saxo Bank ポータルへのログイン画面が表示されます。User ID/Password を入力しログインします（2FA を求められる場合はそれに従ってください）。

<span class="pf-step">7</span> 「**REQUEST LIVE APP**」をクリックします。使用許諾画面が表示されるので、チェックを入れて「**ACCEPT**」をクリックします。

<span class="pf-step">8</span> Request Live Application 画面で以下のように入力してください。

| 項目 | 設定値 |
|---|---|
| **Name** | 任意の名称 |
| **Description** | 任意の説明文 |
| **Redirect URL** | `http://localhost:12321/callback` |
| **Grant Type** | `Code` を選択 |
| **Access Control** | チェックなし（デフォルトのまま） |

<span class="pf-step">9</span> 全ての入力が終わったら「**REQUEST LIVE APPLICATION**」をクリックします。

<span class="pf-step">10</span> 作成した Live App をクリックし、詳細画面へ進みます。**App Key** と **App Secret** のそれぞれの **COPY** ボタンをクリックして控えておきます。

#### 認証トークン作成

<span class="pf-step">1</span> PF Manager の「**Settings**」>「**Saxo Bank OpenAPI**」の各テキストボックスへ、控えた **App Key** と **App Secret** を入力します。**Environment** は `Live` を選択します。

<span class="pf-step">2</span> 「**Save All Settings**」をクリックし、設定を保存します。

<span class="pf-step">3</span> Dashboard 画面へ行き、「**API Data Download & Import**」をクリックします。Saxo Bank データを読み込むタイミングで、Web ブラウザに認証画面が表示されます。

<span class="pf-step">4</span> Saxo Bank ポータルへのログイン画面で User ID/Password を入力してログインします（2FA を求められる場合はそれに従ってください）。

<span class="pf-step">5</span> 認証通過後、Web ブラウザで「**このサイトにアクセスできません**」と表示されます。現在表示されているページの **URL 全てをコピー**し、別ウィンドウでポップアップしている Saxo Bank 認証ダイアログボックスへそのURLをペーストします。

!!! warning "素早く作業してください"
    一定時間が経過すると Web ページがリロードされ、URL が取得できない恐れがあります。この手順は素早く行ってください。

<span class="pf-step">6</span> データのダウンロード＆インポート完了後、Dashboard 画面や Analytics 画面で Saxo Bank のデータが正しく取得できているかを確認してください。

!!! tip "取得できていない場合"
    認証エラーとなっている可能性が高いため、「API Data Download & Import」をクリックする手順に戻って、再度手順を進めてください。

---

### 3. Webull

Webull の各種データはブラウザ版（`https://app.webull.com`）からダウンロードしてください。

=== "保有銘柄（ポジション）"

    <span class="pf-step">1</span> ログイン後、上部メニュー「**Assets**」または「**Account**」をクリックします。

    <span class="pf-step">2</span> 「**Positions（保有銘柄）**」を開きます。

    <span class="pf-step">3</span> 画面右上の **Export / Download / ⬇ アイコン** をクリックし、CSV 形式で保存します。

=== "取引履歴・配当金"

    <span class="pf-step">1</span> ログイン後、「**History / Transaction History**」を選択します。

    <span class="pf-step">2</span> 画面上部で期間指定（例：Custom Date Range）や種類（All / Trades / Dividends など）を選択します。

    <span class="pf-step">3</span> 「**Export**」ボタンをクリックし、CSV で保存します。
