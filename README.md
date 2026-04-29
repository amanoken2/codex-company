# codex-company

OpenAI Codex CLI で仮想会社組織を構築・運営するプラグイン（WIP）。

`/company` を実行すると秘書が窓口となり、あなたの事業に最適化された組織（部署）を構築。日常の運営では、秘書に話しかけるだけで CEO が適切な部署に仕事を振り分けます。

> **Status: 改造作業中** — Claude Code 用の [cc-company](https://github.com/Shin-sibainu/cc-company) を Codex 向けに移植・改変中。

## オリジナル

このプロジェクトは [Shin-sibainu/cc-company](https://github.com/Shin-sibainu/cc-company)（MIT License）をベースにしています。原作者に感謝。

## 計画している主な変更点

- `CLAUDE.md` → `AGENTS.md`（Codex 標準のディレクトリ別文脈ファイル）
- `.claude-plugin/` → `.codex-plugin/` + plugin.json/marketplace.json をCodex形式に
- `AskUserQuestion` 依存箇所を、テキストベースの質問フローに置換
- （その他、運用してみて気に入らない部分を作り変え予定）

## インストール（移植完了後）

```
codex marketplace add amanoken2/codex-company
codex plugin install company@codex-company
```

## コンセプト

```
あなた → 秘書（窓口） → CEO（振り分け） → 各部署
```

- **秘書**: 常に窓口。何でも相談OK。TODO管理、壁打ち、メモ
- **CEO**: 裏方で判断。部署が必要な案件を自動振り分け
- **各部署**: 専門領域のファイル管理を担当

ユーザーは部署を意識する必要なし。秘書に話しかけるだけ。

## 部署一覧

| 部署 | 担当領域 | 常設 |
|------|---------|------|
| 秘書室 | TODO管理、壁打ち、メモ、相談 | はい |
| CEO | 意思決定、部署振り分け | はい |
| レビュー | 週次・月次レビュー | はい |
| PM | プロジェクト進捗、チケット管理 | |
| リサーチ | 市場調査、競合分析、技術調査 | |
| マーケティング | コンテンツ企画、SNS、キャンペーン | |
| 開発 | 技術ドキュメント、設計、デバッグ | |
| 経理 | 請求書、経費、売上管理 | |
| 営業 | クライアント管理、提案書 | |
| クリエイティブ | デザインブリーフ、ブランド管理 | |
| 人事 | 採用管理、チーム管理 | |

## ライセンス

MIT — 原作者 Shin-sibainu に基づく派生作品。詳細は [LICENSE](LICENSE) 参照。
