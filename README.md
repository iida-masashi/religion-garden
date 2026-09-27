# 日本の新宗教・教団構造研究 デジタルガーデン (religion-garden)

> 日本の新宗教・分派・異端教団に関する学術的構造分析ノート群を公開するデジタルガーデン。教理体系、組織構造、歴史的変遷、法廷確定判決、分派系統樹を客観的に記録・公開しています。

🌐 **公開サイト**: <https://iida-masashi.github.io/religion-garden/>

[![Deploy](https://github.com/iida-masashi/religion-garden/actions/workflows/deploy.yml/badge.svg)](https://github.com/iida-masashi/religion-garden/actions/workflows/deploy.yml)

---

## 🏛 リポジトリ構成と公開アーキテクチャ

このリポジトリは、ローカルの **Obsidian Vault** を Source of Truth（真実のソース）とし、**[Quartz v4](https://quartz.jzhao.xyz/)** を介して静的サイトを生成、**GitHub Pages** で世界に向けて高速配信しています。

```mermaid
flowchart LR
    classDef vault fill:#1e3a8a,stroke:#ffffff,stroke-width:2px,color:#ffffff;
    classDef sync fill:#065f46,stroke:#ffffff,stroke-width:2px,color:#ffffff;
    classDef quartz fill:#7f1d1d,stroke:#ffffff,stroke-width:2px,color:#ffffff;
    classDef deploy fill:#374151,stroke:#ffffff,stroke-width:2px,color:#ffffff;

    A["Obsidian Vault<br>(Private / Source of Truth)"]:::vault -->|"_sync_to_quartz_religion.py<br>(差分同期 + 編集ログ除去 + LF変換)"| B["quartz-religion/content<br>(公開用Markdown)"]:::sync
    B -->|"npx quartz build"| C["Quartz v4 Engine<br>(静的HTML/JS生成)"]:::quartz
    C -->|"git push"| D["GitHub Actions<br>Auto Deploy"]:::deploy
    D --> E["GitHub Pages<br>(religion-garden)"]:::deploy
```

---

## 🔬 本研究の特徴と分析規律

本デジタルガーデンは、印象論や二次報道による感情的断罪を排し、以下の厳格な規律に基づいて構築されています：

1. **ファクトベース・一次情報主義の徹底**:
   - 教団自身の公式経典・機関紙・公式発表資料（一次資料）
   - 最高裁判所等の確定判決録、民事・刑事訴訟記録（司法一次情報）
   - 文化庁『宗教年鑑』、警察庁『警察白書』、公安調査庁『内外情勢の回顧と展望』等の公的官公庁統計
2. **10セクション標準分析フォーマットの適用**:
   - 全個別教団ノートを10セクション（概要、Mermaid 11系統樹、指導層・後継、歴史年表、教理・生活規律、事件・最高裁判決、拠点アクセス、出版・関連企業、史料・参考文献、留保事項）で統一。
3. **客観的メタデータによる品質ゲート**:
   - 各ノートのfrontmatterに `confidence`（確度1〜5）および `evidence_type`（典拠種別）を明記し、未検証情報の混入を防止。

---

## 📚 収録コンテンツ概要（全369件）

一次資料アーカイブ（`sources/`）を除く **369件** の公開ノートを体系的に収録しています。

### 主要カテゴリ
- **天理教系（23件）**: 天理教、ほんみち、ほんぶしん、甘露台・おやさま継承抗争、裁判史
- **世界救世教・真光系（29件）**: 世界救世教、崇教真光、神慈秀明会、手かざし浄霊の系譜
- **教派神道十三派（12件）**: 黒住教、金光教、出雲大社教、神理教など明治公認教団
- **法華・日蓮系（16件）**: 創価学会、日蓮正宗、正信会、富士大石寺顕正会、国柱会
- **キリスト教系・異端（18件）**: 世界平和統一家庭連合、エホバの証人、末日聖徒イエス・キリスト教会
- **オウム真理教・後継分派群（事件性団体含む35件）**: オウム真理教、Aleph、ひかりの輪、山田らの集団
- **自己啓発・霊能治病カルト**: ライフスペース、神世界、法の華三法行
- **その他**: 大本系（7）、手かざし系（11）、神道系（31）、仏教系（15）、コミューン系（8）、比較・史論（22）、人物（30）、概念・拠点（33）、周辺領域（79）

---

## 🚀 同期・ビルド・公開ワークフロー

### 1. Vaultからの差分同期（ローカル実行）
```bash
# Vault側でノートを編集・執筆後、同期スクリプトを実行
python D:/Vault/religion/_work/_sync_to_quartz_religion.py
```
* スクリプトが自動的に Mermaid 構文の事前バリデーション、差分ミラーリング、編集ログ（`更新履歴` フィールド等）の安全な除去、および改行コード（LF）正規化を実施します。

### 2. ローカルプレビュー（任意）
```bash
cd C:/Users/iidam/quartz-religion
npx quartz build --serve
# ブラウザで http://localhost:8080 を開いて確認
```

### 3. 本番反映・デプロイ
```bash
cd C:/Users/iidam/quartz-religion
git add -A
git commit -m "feat: content update"
git push
```
* リモートへの `git push` をトリガーとして GitHub Actions が起動し、約1〜2分で GitHub Pages に自動反映されます。

---

## ⚙️ Quartz 設定概要

`quartz.config.ts`:
- **pageTitle**: 日本の新宗教・教団構造研究
- **locale**: ja-JP
- **baseUrl**: iida-masashi.github.io/religion-garden
- **ignorePatterns**: private, templates, .obsidian, _work, *.bak, sources, BACKLOG_*.md

---

## 📜 ライセンス・クレジット

- **Quartz v4 Engine**: MIT License (Created by [jackyzha0](https://github.com/jackyzha0))
- **研究コンテンツ (`content/`)**: 著作権は著者 (iida-masashi) に帰属します。学術引用・参照の際は出典の明記をお願いいたします。
- **個別教団の典拠**: 各ノート末尾の「史料・参考文献」欄および「検証・品質ゲートと留保事項」を参照してください。
