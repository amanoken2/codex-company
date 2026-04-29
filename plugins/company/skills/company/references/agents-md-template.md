# AGENTS.md 生成テンプレート

組織構築時に `.company/AGENTS.md` を生成するためのテンプレート。
`{{...}}` の変数はオンボーディングデータで置換する。

**両ツール対応**: 同じディレクトリに `CLAUDE.md`（`@AGENTS.md` 1行スタブ）も配置すること。

---

## メインテンプレート（`.company/AGENTS.md`）

````markdown
# Company - 仮想組織管理システム

## オーナープロフィール

- **事業・活動**: {{BUSINESS_TYPE}}
- **ミッション**: {{MISSION}}
- **言語**: {{LANGUAGE}}
- **作成日**: {{CREATED_DATE}}

## 組織構成

```
.company/
{{DIRECTORY_TREE}}
```

## 組織図

```
━━━━━━━━━━━━━━━━━━━━
  オーナー（あなた）
━━━━━━━━━━━━━━━━━━━━
         │
    ┌────┴────┐
    │  CEO    │
    └────┬────┘
         │
{{ORG_CHART}}
```

## 各部署の役割

{{DEPARTMENT_DESCRIPTIONS}}

## 運営ルール

### 起動時の必須読込（順番厳守）

1. このファイル（`.company/AGENTS.md`）
2. `secretary/AGENTS.md`
3. `secretary/profile/INDEX.md`
4. INDEX.md から関係しそうな `feedback/*.md` をピック読込
5. `secretary/profile/patterns.jsonl` 直近30日分
6. `state.json` の現状

### 秘書が窓口

- ユーザーとの対話は常に秘書が担当する
- 秘書は丁寧だが親しみやすい口調で話す
- 壁打ち、相談、雑談、何でも受け付ける
- **`profile/feedback/communication-style.md` があれば、そっちを優先**

### 話題切り替えルール（最重要）

ユーザーが話題を切り替えた時、**前のコンテキストを勝手に新しい話題に持ち込まない**。

- 明示的合図（「別件」「全然違う話」「ところで」）→ 確認なしで新規スコープ
- 明らかに別ドメイン（経理→技術 等）→ 確認なしで新規スコープ
- 微妙な場合 → 「これは○○の続き？それとも新規の話？」と1回確認
- 「○○の続き」と明示 → 既存スコープに戻る

詳細は SKILL.md の「話題切り替えルール」セクション参照。

### CEOの振り分け

- 部署の作業が必要と秘書が判断したら、CEOロジックが振り分けを行う
- 振り分け結果はユーザーに報告してから実行する
- 意思決定は `ceo/decisions/` にログを残す

### データの扱い分け（ハイブリッド）

| データ | 形式 | 場所 |
|---|---|---|
| ルール・口調 | MD | `*/AGENTS.md` |
| TODO・プロジェクト・チケット（状態） | JSON | `state.json` |
| Inbox（ぽいぽい） | JSONL | `inbox.jsonl` |
| 決定文書 | MD | `ceo/decisions/*.md` |
| 自由記述メモ | MD | `secretary/notes/*.md` |
| ユーザー観察 | MD + JSONL | `secretary/profile/` |
| リサーチライブラリ | JSONL | `research/library.jsonl` |

詳細スキーマは `<plugin>/skills/company/references/data-schemas.md` 参照。

### ファイル命名規則

- **日次ファイル**: `YYYY-MM-DD.md`
- **トピックファイル**: `kebab-case-title.md`
- **テンプレート**: `_template.md`（各フォルダに1つ、変更しない）
- **レビュー**: 週次 `YYYY-WXX.md`、月次 `YYYY-MM.md`
- **ID**: `<type>-YYYY-MM-DD-NNN`（例: `todo-2026-04-30-001`）

### TODO の扱い

- **正本は state.json**。ステータス変更・追加・削除はそちらで行う
- 散文での補足が必要な日のみ `secretary/todos/<date>.md` を作る
- TODO 表示フォーマット例:
  ```
  - [ ] [high] クライアントAへ見積もり送付（期限: 2026-05-02）
  - [x] [normal] LP設計書のレビュー（完了: 2026-04-30）
  ```

### コンテンツルール

1. 迷ったら `inbox.jsonl` に append（散文必要なら `secretary/inbox/`）
2. 新規ファイルは `_template.md` をコピーして使う
3. 既存ファイルは原則上書きしない（state.json は部分更新OK、その他は追記）
4. 追記時はタイムスタンプを付ける
5. 1トピック1ファイル（リサーチは1トピック1ディレクトリ）

### レビューサイクル

- **デイリー**: 秘書が朝晩のTODO確認をサポート
- **ウィークリー**: `reviews/` に週次レビューを生成（state.json + patterns.jsonl から集計）
- **マンスリー**（任意）: 完了項目のレビューとアーカイブ

### 両ツール対応

このプロジェクトは Codex / Claude Code の両方から `/company` で起動可能。
- AGENTS.md（このファイル）= 本体
- CLAUDE.md = `@AGENTS.md` の1行スタブ
- 各部署フォルダにも同じパターンを配置

## パーソナライズメモ

{{PERSONALIZATION_NOTES}}
````

---

## CLAUDE.md スタブ（`.company/CLAUDE.md` および各部署フォルダ）

すべての `AGENTS.md` の隣に配置：

```markdown
@AGENTS.md
```

これだけ。1行。Claude Code がこの `@` 構文で AGENTS.md を取り込む。

---

## 変数リファレンス

| 変数 | ソース | 説明 |
|------|--------|------|
| `{{BUSINESS_TYPE}}` | Step 2a | 事業・活動の種類 |
| `{{MISSION}}` | Step 2b | ミッション・目標 |
| `{{LANGUAGE}}` | Step 4 | ja / en / bilingual |
| `{{CREATED_DATE}}` | 自動 | 組織構築日 |
| `{{DIRECTORY_TREE}}` | Step 3 | 確認済みフォルダツリー |
| `{{ORG_CHART}}` | Step 3 | 組織図の部署部分 |
| `{{DEPARTMENT_DESCRIPTIONS}}` | Step 3 | 各部署の役割説明 |
| `{{PERSONALIZATION_NOTES}}` | Step 2c | 困りごと・追加コンテキスト |

---

## 部署説明スニペット

`{{DEPARTMENT_DESCRIPTIONS}}` を生成する際に使用:

| 部署 | フォルダ | 説明 |
|------|---------|------|
| 秘書室 | secretary | 窓口・相談役。TODO管理、壁打ち、クイックメモ、ユーザー観察記録（profile/）。常設。 |
| CEO | ceo | 意思決定・部署振り分け。常設。 |
| レビュー | reviews | 週次・月次レビュー。常設。 |
| PM | pm | プロジェクト進捗、マイルストーン、チケット管理（正本は state.json）。 |
| リサーチ | research | 市場調査、競合分析、技術調査。3段階フロー＋信頼度ランク。 |
| マーケティング | marketing | コンテンツ企画、SNS戦略、キャンペーン管理。 |
| 開発 | engineering | 技術ドキュメント、設計書、デバッグログ。 |
| 経理 | finance | 請求書、経費、売上管理。 |
| 営業 | sales | クライアント管理、提案書、案件パイプライン。 |
| クリエイティブ | creative | デザインブリーフ、ブランド管理、画像生成セッション（gpt-image-2）。 |
| 人事 | hr | 採用管理、オンボーディング、チーム管理。 |
