# 日本の新宗教研究デジタルガーデン (religion-garden)

> 日本の新宗教・分派・異端教団に関する研究ノート群を公開するデジタルガーデン。教義比較、系譜、史的経緯を扱う。

🌐 **公開サイト**: <https://iida-masashi.github.io/religion-garden/>

[![Deploy](https://github.com/iida-masashi/religion-garden/actions/workflows/deploy.yml/badge.svg)](https://github.com/iida-masashi/religion-garden/actions/workflows/deploy.yml)

---

## 構成

このリポジトリは **Obsidian Vault** をソースとし、**[Quartz v4](https://quartz.jzhao.xyz/)** を介して静的サイトを生成、**GitHub Pages** で配信する。

```
Obsidian Vault (private, gitignored)    ← Source of truth
       │
       ▼  sync (Vault → quartz/content/) + 編集ログ除去
 _sync_to_quartz.py
       │
       ▼  build
   Quartz v4
       │
       ▼  push → GitHub Actions
 GitHub Pages → iida-masashi.github.io/religion-garden/
```

## コンテンツ概要

天理教系・大本系・世界救世教系・教派神道十三派・法華日蓮系・キリスト教系異端・手かざし系など、日本の新宗教・分派教団を系統別に整理した個別分析ノート群。各ノートはfrontmatterに`confidence`（確度）・`evidence_type`（典拠種別）・出典URLを明記し、検証状態を可視化している。

## 公開フロー

### Vault → 本番反映

```bash
# Vault 編集 (Obsidian で通常作業)

# 同期 + frontmatter fix + 編集ログ除去
cd <vault>/_work && uv run python _sync_to_quartz_religion.py

# (任意) ローカルプレビュー
cd <this-repo> && npx quartz build --serve
# → http://localhost:8080

# 公開
cd <this-repo> && git add -A && git commit -m "sync" && git push
```

`git push` で **GitHub Actions が自動 build + deploy**(約 1〜2 分)。

---

## 同期パイプラインの構成

| スクリプト | 役割 |
|---|---|
| `_sync_to_quartz_religion.py` | オーケストレータ(mirror copy + 編集ログ除去 + 後段処理) |
| `_strip_edit_logs.py` | 公開用コピーからのみ、frontmatter`更新履歴:`と編集ログ系`[!danger]`ブロックを除去(Vault本体は無編集) |

(スクリプトは Vault 側の private 領域で管理、このリポジトリには含めない)

### 公開対象の選別

**mirror 同期**: `07_分派・異端研究/`、`08_教会・系統研究/`（`sources/`サブフォルダを除く）

**意図的に除外**: `sources/`（一次資料の変換Markdown）、`scratch/`、`templates/`、`Digital Garden Progress.md`、`note_ichinomoto185/`（第三者note.comアーカイブ）、`東京都宗教法人名簿*.md`（一括名簿の再配布）

### 編集ログの除去について

このVaultは検証履歴の透明性を保つため、本文中に「以前の記述が誤りだった」という訂正経緯を記録する運用をしている。これは編集作業上は有用だが、読者向けの公開サイトには不要なため、公開用コピー生成時にのみ以下を機械的に除去する（Vault本体のノートはそのまま維持）:

- frontmatterの`更新履歴:`フィールド
- 本文中の`[!danger]`calloutのうち、`Gemini編集`・`一括編集`・`別AI`・`独立検証Agent`等の編集プロセス語彙を含むブロック（事件・不祥事の批判的検証など実質コンテンツの`[!danger]`は保持する）

確定した事実（confidence・evidence_type・出典URL等）は除去対象に含めず、そのまま公開する。

---

## カスタマイズ

`quartz.config.ts`:

- `pageTitle`: 日本の新宗教研究
- `locale`: ja-JP
- `baseUrl`: iida-masashi.github.io/religion-garden
- `ignorePatterns`: private、templates、.obsidian、_work、*.bak、BACKLOG_*.md

### 依存関係更新の注意

Dependabot の一括更新 PR はメジャーバージョンアップを含むことがあり、`npm run check`(型チェック)通過後も実ビルド(`npx quartz build`)が壊れる場合がある。マージ前に必ず実ビルドまで確認すること。

---

## クレジット

- **Quartz v4**: <https://quartz.jzhao.xyz/> by [jackyzha0](https://github.com/jackyzha0) (MIT License)
- **コンテンツ**: 日本の新宗教研究に関する研究ノート(iida-masashi)
- 各論ノートの典拠は本文の参考文献欄を参照

---

## ライセンス

- **Quartz コード本体**: MIT License(`LICENSE.txt`)
- **コンテンツ(`content/`配下のノート)**: 著作権は著者(iida-masashi)に帰属。引用・参照時は出典の明記をお願いします。
