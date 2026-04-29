# データスキーマ定義

`.company/` 配下で使用する **JSON / JSONL** データのスキーマを定義する。
MD（散文）で扱うべきものとは明確に分離し、構造化データはここに集約する。

---

## 設計方針

| データの性質 | 形式 | 例 |
|---|---|---|
| 高頻度更新・状態を持つ | **JSON** | TODO のチェック ON/OFF、プロジェクトのステータス変更 |
| 追記専用・時系列 | **JSONL** | inbox の投げ込み、AI観察パターン、リサーチライブラリ |
| 散文・自由記述・人間が読みたい | **MD** | 壁打ちノート、決定文書、議事録 |

JSON の部分更新が必要な時は、AI が一度全体を読んで該当キーだけ更新して書き戻す。スキーマが破壊されないよう、**操作前後で必ず JSON.parse / json.tool で構文チェック**する。

---

## 1. `.company/state.json`

組織全体の状態データ。TODO・プロジェクト・チケットなど、頻繁に更新される構造化データを集約。

### スキーマ

```json
{
  "version": 1,
  "updated_at": "YYYY-MM-DDTHH:MM:SS+09:00",

  "todos": [
    {
      "id": "todo-2026-04-30-001",
      "title": "クライアントAへ見積もり送付",
      "status": "open",
      "priority": "high",
      "created": "2026-04-30",
      "due": "2026-05-02",
      "completed": null,
      "department": "secretary",
      "tags": ["urgent"],
      "linked_project": null,
      "notes": ""
    }
  ],

  "projects": [
    {
      "id": "proj-001",
      "name": "新規LP",
      "status": "in-progress",
      "owner": "pm",
      "created": "2026-04-15",
      "deadline": "2026-05-31",
      "milestones": [
        {"name": "デザイン確定", "due": "2026-04-22", "status": "completed"},
        {"name": "実装完了", "due": "2026-05-15", "status": "in-progress"},
        {"name": "公開",     "due": "2026-05-31", "status": "pending"}
      ],
      "linked_departments": ["pm", "marketing", "engineering"],
      "notes": "見積OK、契約書受領済み"
    }
  ],

  "tickets": [
    {
      "id": "tkt-2026-04-30-001",
      "project_id": "proj-001",
      "title": "ヒーロー画像制作",
      "status": "open",
      "priority": "normal",
      "assignee": "creative",
      "created": "2026-04-30",
      "due": "2026-05-08",
      "completed_at": null
    }
  ],

  "scopes": [
    {
      "id": "scope-001",
      "topic": "サーバ監視 NewRelic設定",
      "active": true,
      "started": "2026-04-29",
      "last_touched": "2026-04-30",
      "summary": "ecn-v-01 監視設定完了、アラートまで通った"
    }
  ]
}
```

### 操作ルール

- 新規追加: `todos[]` / `projects[]` / `tickets[]` に append
- ステータス変更: 該当 ID を見つけて `status` フィールドだけ更新
- 削除: 物理削除より `status: "archived"` 推奨
- ID 命名: kebab-case、日付ベース（`todo-YYYY-MM-DD-NNN`）
- 更新時は必ず `updated_at` を現在時刻にする

### 初期化

```json
{
  "version": 1,
  "updated_at": "{{NOW_ISO}}",
  "todos": [],
  "projects": [],
  "tickets": [],
  "scopes": []
}
```

### ステータス定義

- TODO: `open` / `in-progress` / `done` / `archived`
- プロジェクト: `planning` / `in-progress` / `review` / `completed` / `archived`
- マイルストーン: `pending` / `in-progress` / `completed`
- チケット: `open` / `in-progress` / `done` / `blocked`

### 優先度

- `critical` / `high` / `normal` / `low`

---

## 2. `.company/inbox.jsonl`

ユーザーが「ぽいぽい投げる」キャプチャを追記専用で記録。1行 = 1キャプチャ。

### スキーマ

```jsonl
{"ts":"2026-04-30T15:23:45+09:00","type":"capture","text":"newrelic alert設定したい","tags":["infra"],"processed":false}
{"ts":"2026-04-30T15:30:12+09:00","type":"idea","text":"車種選択ガジェットM5Stackで","tags":["product","hardware"],"processed":false}
{"ts":"2026-04-30T16:01:00+09:00","type":"todo","text":"クライアントBへメール返信","tags":["sales"],"processed":true,"linked_id":"todo-2026-04-30-002"}
```

### フィールド

| フィールド | 型 | 必須 | 説明 |
|---|---|---|---|
| `ts` | string (ISO8601) | yes | タイムスタンプ |
| `type` | string | yes | `capture` / `idea` / `todo` / `note` / `question` |
| `text` | string | yes | 本文 |
| `tags` | array | no | 任意のタグ |
| `processed` | bool | yes | 後で整理した？（state.json や notes/ に昇格させたか） |
| `linked_id` | string | no | 昇格先の ID（todo-xxx 等） |

### 操作ルール

- 追記のみ。**既存行は絶対に編集しない**
- 整理時は `processed: true` への変更も追記専用にしたいので、**専用の "processed" レコード**を追加する形が安全：
  ```jsonl
  {"ts":"...","type":"meta","action":"processed","target_ts":"2026-04-30T15:23:45+09:00","linked_id":"todo-..."}
  ```
- AIが「inbox整理」を実行する時は、すべての行を読み、processed=false のものを表示する

### 初期化

空ファイル（0バイト）でOK。

---

## 3. `.company/secretary/profile/INDEX.md`

ユーザー観察記録の全体インデックス。**起動時に必ず読む**。

### フォーマット（MDだが構造化）

```markdown
# Profile Index

最終更新: YYYY-MM-DD

## basics
- [basics.md](basics.md) - 職業・関心領域・基本人物像

## feedback
- [feedback/communication-style.md](feedback/communication-style.md) [priority:high] - 簡潔・結論先出し、褒め禁止
- [feedback/topic-switching.md](feedback/topic-switching.md) [priority:high] - 話題切り替え時は前ctx持ち込まない
- [feedback/implementation-confirmation.md](feedback/implementation-confirmation.md) [priority:critical] - 実装前に必ず方針確認

## patterns
- [patterns.jsonl](patterns.jsonl) - 観察パターンログ（直近30日参照推奨）
```

### 更新ルール

- feedback/*.md を新規作成・削除した時に必ず更新
- priority タグを必ず付ける（`[priority:critical|high|normal|low]`）

---

## 4. `.company/secretary/profile/basics.md`

ユーザーの基本人物像。変わりにくい情報。

### フォーマット

```markdown
---
name: ""
role: ""
domain: []
language: ja
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# 基本人物像

- **職業/役割**: 
- **主たる関心**: 
- **ドメイン専門性**: 
- **意思決定スタイル**: 
- **言語/口調**: 
- **その他特記**: 
```

---

## 5. `.company/secretary/profile/feedback/*.md`

1ファイル＝1ルール。

### スキーマ

```markdown
---
rule: ルールの一行サマリ（必須）
priority: critical | high | normal | low
why: なぜこのルールができたか（必須）
how_to_apply: 具体的にいつ・どう適用するか（必須）
created: YYYY-MM-DD
last_confirmed: YYYY-MM-DD
incident: "（任意）きっかけになった事件の記録"
---

（必要なら散文で補足）
```

### 命名

- `feedback/<short-kebab-case>.md`
- 例: `communication-style.md` / `implementation-confirmation.md` / `git-workflow.md`

### 削除

ユーザーが「もう要らない」と言ったら物理削除。INDEX.md からも消す。

---

## 6. `.company/secretary/profile/patterns.jsonl`

AIが観察したユーザーの行動パターン。追記専用。

### スキーマ

```jsonl
{"date":"2026-04-30","obs":"話題を頻繁に切り替える","confidence":"high","scope":"global","sample_count":4}
{"date":"2026-04-30","obs":"設計議論を長めに好む","confidence":"medium","scope":"topic:codex-company"}
{"date":"2026-04-29","obs":"夜間（22時以降）の応答スタイルが砕ける","confidence":"low","scope":"global"}
```

### フィールド

| フィールド | 型 | 必須 | 説明 |
|---|---|---|---|
| `date` | string | yes | 観察日 |
| `obs` | string | yes | 観察内容（短く） |
| `confidence` | string | yes | `high` / `medium` / `low` |
| `scope` | string | yes | `global` または `topic:<name>` |
| `sample_count` | int | no | 同パターン累積観察回数 |

### 格上げルール

同じ obs が **3回以上 high confidence で観察された場合**、ユーザーに格上げ提案：

> 「最近、○○というパターンを何度か観察しました。これをルールとして profile/feedback/ に登録しましょうか？」

承諾されたら `feedback/*.md` を新規作成し、INDEX.md を更新。

---

## 7. `.company/research/library.jsonl`

リサーチ部署が累積する情報源ライブラリ。詳細は `departments.md` のリサーチセクション参照。

### スキーマ（抜粋）

```jsonl
{"id":"src-001","added":"2026-04-30","topic_id":"topic-001","url":"https://...","title":"...","credibility":"A","summary":"...","tags":["aws","cost"]}
```

`credibility` は A/B/C/D の4段階（A: 公式・一次資料、B: 評判メディア、C: 個人ブログ、D: SNS掲示板）。

---

## 8. データ操作のベストプラクティス

### JSON 編集時

```bash
# 安全な操作パターン（AIが Bash 経由で行う場合）
python3 -c "import json; d=json.load(open('.company/state.json')); d['todos'].append({...}); d['updated_at']='...'; json.dump(d, open('.company/state.json','w'), indent=2, ensure_ascii=False)"
```

または Read → Edit ツールでピンポイント書き換え。**書き込み後は必ず構文チェック**：

```bash
python3 -m json.tool .company/state.json > /dev/null && echo OK || echo BROKEN
```

### JSONL 追記時

```bash
echo '{"ts":"...","type":"capture","text":"..."}' >> .company/inbox.jsonl
```

**改行を忘れない**（最後の `>>` だと改行付きで追記される）。AIが `Edit` で開かないこと（既存行を壊す危険）。

### MD 編集時

通常通り Read → Edit / Write。タイムスタンプ付き追記の場合は末尾に **`## YYYY-MM-DD HH:MM`** 見出しを追加するパターン推奨。

---

## 9. 部分更新が壊れた時の復旧

JSON が壊れた場合：

1. 直近の git 履歴があるなら `git checkout HEAD -- .company/state.json`
2. 無ければ AI が手動で再構築
3. 構造的に壊れた感じなら、**バックアップ運用を推奨**：
   ```bash
   cp .company/state.json .company/.backups/state-$(date +%Y%m%d-%H%M%S).json
   ```
   重要な更新前に AI が自動でバックアップを取る運用ルール化も可。
