# REPORT — レポート・AI要約

現在の資産状況を多角的に分析し、AI からの客観的なアドバイスを確認する画面です。

![REPORT画面](images/image6.png)

---

## 主な機能と動作

### コントロールバー

分析期間とグループ分け（セクター別か、MyGroup か）を選択し「**Update Report**」を押すことで、指定した条件でレポートを再生成します。

| 設定項目 | 選択肢 |
|---|---|
| 分析期間 | 1W / 1M / 3M / YTD / 1Y / ALL |
| グループ分け | セクター別 / MyGroup（カスタムグループ） |

---

### 各種チャート

資産状況を以下のチャートで視覚的に表示します。[Groups](01_home.md#groups) サブ画面で設定したオリジナル分類もここで活きてきます。

<div class="pf-card-grid">
  <div class="pf-card">
    <div class="pf-card-icon">📈</div>
    <h4>資産推移</h4>
    <p>ポートフォリオ全体の評価額の時系列変化を表示。</p>
  </div>
  <div class="pf-card">
    <div class="pf-card-icon">💰</div>
    <h4>配当予測</h4>
    <p>保有銘柄から見込まれる配当収入の見通しを表示。</p>
  </div>
  <div class="pf-card">
    <div class="pf-card-icon">🎯</div>
    <h4>リスクレーダー</h4>
    <p>集中度・ボラティリティなどのリスク指標をレーダーチャートで可視化。</p>
  </div>
</div>

!!! tip "AI要約の活用"
    レポートの末尾には AI による客観的なポートフォリオ評価が表示されます。感情を排した第三者視点のアドバイスとして参考にしてください。
