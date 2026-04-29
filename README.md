# codex-company

**Codex / Claude Code 両対応**の仮想会社組織プラグイン。

`/company` を実行すると秘書が窓口となり、あなたの事業に最適化された組織（部署）を構築。日常の運営では、秘書に話しかけるだけで CEO が適切な部署に仕事を振り分けます。会話を重ねるほど秘書がユーザーの好み・スタイルを学習します。

## オリジナル

このプロジェクトは [Shin-sibainu/cc-company](https://github.com/Shin-sibainu/cc-company)（MIT License）をベースにしています。原作者に感謝。

## オリジナルからの主な変更点

### Codex 対応 + 両ツール対応
- `CLAUDE.md` → `AGENTS.md`（Codex 標準）に切り替え
- `CLAUDE.md` は `@AGENTS.md` の1行スタブとして配置 → Claude Code もそのまま動く
- `.claude-plugin/` に加え `.codex-plugin/` のマニフェストも追加
- `AskUserQuestion` 依存箇所をテキストベースの質問フローに置換

### AIファースト・ハイブリッドデータ構造
全部 MD で書いていた状態データを、用途に応じて使い分け：
- **MD**：ルール・口調・散文ノート（`*/AGENTS.md`、`notes/`、`decisions/`）
- **JSON**：状態を持つデータ（`state.json`：TODO・プロジェクト・チケット）
- **JSONL**：追記専用ログ（`inbox.jsonl`、`patterns.jsonl`、`research/library.jsonl`）

### 話題切り替えルール（最重要）
ユーザーが話題を切り替えた時、**前のコンテキストを勝手に新しい話題に持ち込まない**3段階ルールを実装。
（経理の話 → 突然「サーバ買う」→ ❌「年度予算に組み込みますか？」を防ぐ）

### ユーザー観察記録（profile/）
秘書が会話を重ねながらユーザーの好み・スタイル・過去のフィードバック・行動パターンを `secretary/profile/` に蓄積：
- `basics.md`：基本人物像
- `feedback/*.md`：1ファイル＝1ルール（priority・why・how_to_apply 付き）
- `patterns.jsonl`：AIが観察した行動パターン（追記専用）

### research部署を厚く再設計
- 3段階フロー（Broad Scan → Focused → Synthesis）
- 情報源信頼度ランク A/B/C/D
- 1トピック1ディレクトリ、iterations/ で深堀り履歴
- `final.md` には TL;DR / 確信度 / 反証可能性 必須

### 画像生成セッション（クリエイティブ部署）
`gpt-image-2`（マルチターン編集対応）でのデザイン制作セッションを管理：
- `sessions/<YYYY-MM-DD-slug>/` でセッション単位管理
- `meta.json` で iteration 履歴・コスト記録
- `tools/imagegen.sh` / `tools/imagedit.sh` 同梱
- 参照画像最大10枚に対応

## インストール

### Claude Code
```
/plugin marketplace add amanoken2/codex-company
/plugin install company@codex-company
```

### Codex
```
codex marketplace add amanoken2/codex-company
codex plugin install company@codex-company
```

## コンセプト

```
あなた → 秘書（窓口） → CEO（振り分け） → 各部署
```

- **秘書**: 常に窓口。何でも相談OK。TODO管理、壁打ち、メモ、ユーザー観察
- **CEO**: 裏方で判断。部署が必要な案件を自動振り分け
- **各部署**: 専門領域のファイル管理を担当

ユーザーは部署を意識する必要なし。秘書に話しかけるだけ。

## 部署一覧

| 部署 | 担当領域 | 常設 |
|------|---------|------|
| 秘書室 | TODO管理、壁打ち、メモ、相談、**ユーザー観察記録** | はい |
| CEO | 意思決定、部署振り分け | はい |
| レビュー | 週次・月次レビュー | はい |
| PM | プロジェクト進捗、チケット管理（state.json） | |
| **リサーチ** | **3段階フロー＋信頼度ランク付き調査** | |
| マーケティング | コンテンツ企画、SNS、キャンペーン | |
| 開発 | 技術ドキュメント、設計、デバッグ | |
| 経理 | 請求書、経費、売上管理 | |
| 営業 | クライアント管理、提案書 | |
| **クリエイティブ** | デザインブリーフ、ブランド管理、**画像生成セッション（gpt-image-2）** | |
| 人事 | 採用管理、チーム管理 | |

## ファイル構成

```
codex-company/
├── .claude-plugin/marketplace.json    # Claude Code 用
├── .codex-plugin/marketplace.json     # Codex 用
├── plugins/company/
│   ├── .claude-plugin/plugin.json
│   ├── .codex-plugin/plugin.json
│   └── skills/company/
│       ├── SKILL.md                    # 中核ロジック
│       └── references/
│           ├── departments.md          # 部署別テンプレ + AGENTS.md
│           ├── agents-md-template.md   # .company/AGENTS.md 生成用
│           └── data-schemas.md         # state.json / inbox.jsonl / profile/ スキーマ
├── README.md
└── LICENSE
```

## 生成される `.company/` の構造

```
.company/
├── AGENTS.md                       # 組織全体ルール（本体）
├── CLAUDE.md                       # @AGENTS.md（スタブ）
├── state.json                      # TODO・プロジェクト・チケット
├── inbox.jsonl                     # 追記専用キャプチャログ
├── secretary/
│   ├── AGENTS.md / CLAUDE.md
│   ├── profile/                    # ユーザー観察記録
│   │   ├── INDEX.md
│   │   ├── basics.md
│   │   ├── feedback/
│   │   └── patterns.jsonl
│   ├── inbox/ todos/ notes/
├── ceo/
│   ├── AGENTS.md / CLAUDE.md
│   └── decisions/
├── reviews/
└── [選択した部署]/
```

## ライセンス

MIT — 原作者 [Shin-sibainu](https://github.com/Shin-sibainu) の cc-company に基づく派生作品。詳細は [LICENSE](LICENSE) 参照。
