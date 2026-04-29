# 部署別テンプレート集

組織構築時に各部署フォルダへ配置する `_template.md`、`AGENTS.md`、および補助スクリプトのテンプレート集。
言語設定に応じて日本語版または英語版を使い分ける（このファイルは日本語版）。

**両ツール対応**: 各 `AGENTS.md` の隣には必ず `CLAUDE.md` (`@AGENTS.md` 1行スタブ) を配置する。

---

# 部分1: 各部署のファイルテンプレート

---

## 1. 秘書室（secretary）※常設

### デイリーTODO（secretary/todos/_template.md）

> Note: TODO の正本は `.company/state.json` の `todos[]`。このMDファイルは「散文での補足が必要な日」「日次ふりかえり」用の補助ノート。

```markdown
---
date: "{{YYYY-MM-DD}}"
type: daily-note
---

# {{YYYY-MM-DD}} ({{DAY_OF_WEEK}})

## 今日の目標（散文）


## メモ・振り返り
- 

## 明日に持ち越したいこと
- 
```

### Inbox 散文版（secretary/inbox/_template.md）

> Note: 通常のクイックキャプチャは `.company/inbox.jsonl` に1行 append。このMDは「散文で残したいキャプチャ」用。

```markdown
---
date: "{{YYYY-MM-DD}}"
type: inbox-prose
---

# Inbox - {{YYYY-MM-DD}}

## キャプチャ

- **{{HH:MM}}** | 
```

### 壁打ち・相談メモ（secretary/notes/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
topic: ""
type: note
tags: []
linked_scope: ""
---

# [相談テーマ]

## 背景・きっかけ
何について考えたい？

## 議論・思考メモ
- 

## 結論・ネクストアクション
- [ ] 
```

### 部署トップ（secretary/_template.md）

```markdown
---
type: department
name: 秘書室
role: 窓口・相談役・タスク管理・ユーザー観察
---

# 秘書室

何でもお気軽にどうぞ。TODO管理、壁打ち、メモ、何でも承ります。

## サブフォルダ
- `profile/` - ユーザー観察記録（重要・起動時必読）
- `inbox/` - 散文キャプチャ（通常は inbox.jsonl 使用）
- `todos/` - 日次散文ノート（通常は state.json 使用）
- `notes/` - 壁打ち・相談メモ
```

### Profile INDEX（secretary/profile/INDEX.md）

```markdown
# Profile Index

最終更新: {{YYYY-MM-DD}}

## basics
- [basics.md](basics.md) - 基本人物像

## feedback
（feedback/ 配下のファイルを追加するたびにここに登録）

## patterns
- [patterns.jsonl](patterns.jsonl) - 観察パターンログ
```

### Profile basics（secretary/profile/basics.md）

```markdown
---
name: ""
role: ""
domain: []
language: ja
created: "{{YYYY-MM-DD}}"
updated: "{{YYYY-MM-DD}}"
---

# 基本人物像

- **職業/役割**: {{BUSINESS_TYPE}}
- **主たる関心**: {{MISSION}}
- **ドメイン専門性**: 
- **意思決定スタイル**: 
- **言語/口調**: {{LANGUAGE}}
- **その他特記**: {{PERSONALIZATION_NOTES}}
```

### Profile feedback テンプレ（secretary/profile/feedback/_template.md）

```markdown
---
rule: ""
priority: normal
why: ""
how_to_apply: ""
created: "{{YYYY-MM-DD}}"
last_confirmed: "{{YYYY-MM-DD}}"
incident: ""
---

（必要なら散文補足）
```

---

## 2. CEO ※常設

### 意思決定ログ（ceo/decisions/_template.md）

```markdown
---
date: "{{YYYY-MM-DD}}"
decision: ""
departments: []
status: decided
linked_scope: ""
---

# 意思決定: [タイトル]

## 背景
何が起きた？何が求められた？

## 判断内容
何を決めた？

## 振り分け先
| 部署 | 指示内容 |
|------|---------|
|      |         |

## 理由
なぜこの判断？

## フォローアップ
- [ ] 
```

---

## 3. レビュー ※常設

### 週次レビュー（reviews/_template.md）

```markdown
---
week: "{{YYYY}}-W{{WW}}"
period: "{{START_DATE}} ~ {{END_DATE}}"
type: weekly-review
---

# 週次レビュー: {{YYYY}}-W{{WW}}

## 完了したタスク（state.jsonから抽出）
- 

## 各部署の動き

### 秘書室
- 

### PM
- 

### その他
- 

## 観察パターンの動き（patterns.jsonl から）
- 

## うまくいったこと
- 

## 改善できること
- 

## 学び・気づき
- 

## 来週の目標
- [ ] 

## 持ち越し（state.jsonから抽出）
- 
```

---

## 4. PM（プロジェクト管理）

> Note: プロジェクトとチケットの正本は `.company/state.json`。このMDファイルは「散文で残したい設計議論・背景」用の補助。

### 部署トップ（pm/_template.md）

```markdown
---
type: department
name: PM
role: プロジェクト進捗・マイルストーン・チケット管理
---

# PM（プロジェクト管理）

プロジェクトの立ち上げから完了まで管理します。
構造化データは state.json、補足散文はこのフォルダ内に。

## サブフォルダ
- `projects/` - プロジェクトごとの散文ノート（背景・議論・決定経緯）
- `tickets/` - 散文タスクノート（複雑な仕様や設計議論用）
```

### プロジェクト散文ノート（pm/projects/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
project_id: ""
title: ""
---

# プロジェクト: [名前]

> このファイルは散文での背景・議論用。ステータス・マイルストーン・期限は state.json を参照。

## 概要
このプロジェクトは何？

## ゴール
何を達成する？

## 背景・経緯
なぜ立ち上げた？誰が言い出した？

## 関連部署とその役割
- 

## 設計議論・決定の経緯
- 

## 懸念・リスク
- 

## メモ
- 
```

### チケット散文ノート（pm/tickets/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
ticket_id: ""
project_id: ""
---

# [チケットタイトル]

> このファイルは散文ノート。ステータス・優先度・担当は state.json を参照。

## 内容
何をする？

## 完了条件
- [ ] 

## 設計・実装メモ
- 

## メモ
- 
```

---

## 5. リサーチ（research）★ 厚く再設計

> 「ちゃんとした調査」のための部署。3段階フロー（Broad Scan → Focused → Synthesis）と情報源信頼度ランク（A-D）を厳格に運用する。

### 部署トップ（research/_template.md）

```markdown
---
type: department
name: リサーチ
role: 市場調査・競合分析・技術調査
methodology: 3-stage (Broad → Focused → Synthesis)
credibility_grades: A/B/C/D
---

# リサーチ

調査・分析を担当します。

## サブフォルダ
- `topics/` - 調査トピック（1トピック1ディレクトリ）
- `library/` - 再利用可能な参照資料（ブックマーク的）
- `archive/` - 完了/放棄した過去トピック

## 累積データ
- `library.jsonl` - 全情報源の累積ライブラリ（追記専用、横断検索用）
```

### 調査トピックディレクトリ構成

```
research/topics/<YYYY-MM-DD-topic-slug>/
├── brief.md          # 何を知りたいか・なぜ
├── sources.jsonl     # このトピックで参照したソース（信頼度付き）
├── notes.md          # 作業ノート（自由記述）
├── iterations/       # 調査を深堀りした回ごとの記録
│   ├── 01-broad.md
│   ├── 02-focused.md
│   └── 03-deepdive.md
└── final.md          # 最終synthesis（決定版）
```

### Brief（research/topics/_template/brief.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
topic_id: ""
title: ""
status: planning
priority: normal
deadline: ""
requester: ""
linked_scope: ""
tags: []
---

# 調査ブリーフ: [タイトル]

## 知りたいこと（核となる問い）
（具体的に。曖昧なら最初に絞る）

## なぜ知りたいか
背景・動機・意思決定への接続

## スコープ
- **対象期間**: 
- **対象市場/領域**: 
- **対象外**: （除外条件）

## 成功基準
何が分かれば調査完了？

## 期限
- 中間共有: 
- 最終納品: 

## 想定アウトプット
- [ ] TL;DR（3行以内）
- [ ] 詳細レポート（final.md）
- [ ] 関連部署への共有
- [ ] state.json への反映
```

### Sources（research/topics/_template/sources.jsonl）

```jsonl
{"id":"src-001","added":"2026-04-30","stage":"broad","url":"https://...","title":"...","credibility":"A","summary":"3行要約","relevance":"high","tags":["aws","cost"]}
{"id":"src-002","added":"2026-04-30","stage":"focused","url":"https://...","title":"...","credibility":"B","summary":"...","relevance":"medium","tags":["aws"]}
```

### Notes（research/topics/_template/notes.md）

```markdown
---
topic_id: ""
type: working-notes
---

# 作業ノート

> 自由記述。考察・仮説・気付き・整理。final.md とは別に「思考の過程」を残す場所。

## {{YYYY-MM-DD}}
- 
```

### Iteration 01: Broad Scan（research/topics/_template/iterations/01-broad.md）

```markdown
---
stage: broad
date: "{{YYYY-MM-DD}}"
duration_minutes: 0
---

# Broad Scan

> 全体像を掴む段階。広く浅く・3〜5ソース。「何が論点か」を見つけるのが目的。
> このステージは結論を出さない。

## 対象ソース
- src-001: 
- src-002: 
- src-003: 

## 分かったこと（事実）
- 

## まだ分からないこと
- 

## 次に絞り込みたい論点
1. 
2. 

## 次のアクション
- [ ] Focused stage に進む
```

### Iteration 02: Focused（research/topics/_template/iterations/02-focused.md）

```markdown
---
stage: focused
date: "{{YYYY-MM-DD}}"
duration_minutes: 0
---

# Focused

> Broad Scan で見つけた論点を深堀り。重要そうな2〜3論点に絞り、5〜10ソースを当たる。
> A/B 信頼度のソースを優先。

## 絞り込んだ論点
1. 
2. 

## 論点1: ...

### 主要ソース
- src-004 (A): 
- src-005 (B): 

### 仮説
- 

### 検証結果
- 

## 論点2: ...
（同上）

## 確信度
- 論点1: high / medium / low
- 論点2: high / medium / low

## まだ未解決
- 

## 次のアクション
- [ ] 必要なら 03-deepdive
- [ ] 不要なら final.md へ
```

### Final（research/topics/_template/final.md）

```markdown
---
topic_id: ""
title: ""
completed: "{{YYYY-MM-DD}}"
confidence: medium
status: completed
---

# 最終レポート: [タイトル]

## TL;DR（3行以内）
1. 
2. 
3. 

## 結論
何が分かったか。

## 確信度
**high / medium / low**

理由: 
- A/B 信頼度ソースが何件揃ったか
- 反対意見・ノイズはあったか

## 反証可能性
**何が分かれば、この結論は変わる？**
- 

## 不明点・次に調べるべきこと
- [ ] 

## 関連する意思決定
- ceo/decisions/... に紐付け（あれば）

## 引用ソース（信頼度ランク順）

### A: 公式ドキュメント・一次資料
- src-001: [タイトル](URL)

### B: 評判のあるメディア・専門ブログ
- src-004: [タイトル](URL)

### C: 個人ブログ・Q&Aサイト
- src-007: [タイトル](URL)

### D: 掲示板・SNS（参考程度）
- src-009: [タイトル](URL)

## 関連部署への共有
- [ ] 共有済み: 
```

### Library（research/library.jsonl）

```jsonl
{"id":"lib-001","added":"2026-04-30","topic_id":"topic-001","url":"https://aws.amazon.com/...","title":"AWS Pricing","credibility":"A","summary":"AWS公式の料金体系まとめ","tags":["aws","pricing"]}
```

> 全トピックを横断する累積ライブラリ。同じソースが複数トピックで再利用される時に便利。

---

## 6. マーケティング

### 部署トップ（marketing/_template.md）

```markdown
---
type: department
name: マーケティング
role: コンテンツ企画・SNS戦略・集客
---

# マーケティング

コンテンツ企画と集客を担当します。

## サブフォルダ
- `content-plan/` - コンテンツ企画
- `campaigns/` - キャンペーン管理
```

### コンテンツ企画（marketing/content-plan/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
platform: ""
status: draft
publish_date: ""
tags: []
---

# [コンテンツタイトル]

## プラットフォーム
ブログ / YouTube / SNS / その他

## ターゲット
誰に向けて？

## 構成
1. 
2. 
3. 

## キーメッセージ


## 下書き


## ステータス
- [ ] 構成
- [ ] 下書き
- [ ] レビュー
- [ ] 公開
```

### キャンペーン（marketing/campaigns/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
campaign: ""
status: planning
period: ""
---

# キャンペーン: [名前]

## 目的
何を達成する？

## ターゲット
- 

## チャネル
- 

## 予算
- 

## KPI
| 指標 | 目標 | 実績 |
|------|------|------|
|      |      |      |

## 振り返り
- 
```

---

## 7. 開発（engineering）

### 部署トップ（engineering/_template.md）

```markdown
---
type: department
name: 開発
role: 技術ドキュメント・設計・デバッグ
---

# 開発

技術的なドキュメントと設計を管理します。

## サブフォルダ
- `docs/` - 技術ドキュメント・設計書
- `debug-log/` - デバッグ・バグ調査ログ
```

### 技術ドキュメント（engineering/docs/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
topic: ""
type: technical-doc
tags: []
---

# [ドキュメントタイトル]

## 概要


## 設計・方針


## 詳細


## 参考
- 
```

### デバッグログ（engineering/debug-log/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
status: open
tags: []
---

# [バグ・問題のタイトル]

## 症状
何が起きている？

## 期待する動作


## 再現手順
1. 

## 調査

### 仮説
- 

### 発見
- 

## 解決策
- 

## 再発防止
- 
```

---

## 8. 経理（finance）

### 部署トップ（finance/_template.md）

```markdown
---
type: department
name: 経理
role: 請求書・経費・売上管理
---

# 経理

お金周りを管理します。

## サブフォルダ
- `invoices/` - 請求書
- `expenses/` - 経費
```

### 請求書（finance/invoices/_template.md）

```markdown
---
date: "{{YYYY-MM-DD}}"
client: ""
amount: 0
status: unpaid
due_date: ""
---

# 請求書: [クライアント名] - {{YYYY-MM-DD}}

## 明細
| 項目 | 数量 | 単価 | 小計 |
|------|------|------|------|
|      |      |      |      |

## 合計


## 支払い状況
- [ ] 送付済み
- [ ] 入金確認済み
```

### 経費（finance/expenses/_template.md）

```markdown
---
date: "{{YYYY-MM-DD}}"
category: ""
amount: 0
---

# 経費: [概要]

## 詳細
| 日付 | 項目 | カテゴリ | 金額 | メモ |
|------|------|---------|------|------|
|      |      |         |      |      |

## 合計

```

---

## 9. 営業（sales）

### 部署トップ（sales/_template.md）

```markdown
---
type: department
name: 営業
role: クライアント管理・提案書・案件パイプライン
---

# 営業

クライアントとの関係を管理します。

## サブフォルダ
- `clients/` - クライアント情報
- `proposals/` - 提案書
```

### クライアント（sales/clients/_template.md）

```markdown
---
client: ""
created: "{{YYYY-MM-DD}}"
status: active
---

# クライアント: [名前]

## 連絡先
- 名前: 
- メール: 
- 会社: 

## 案件履歴
| 案件 | 期間 | 金額 | 状態 |
|------|------|------|------|
|      |      |      |      |

## コミュニケーション履歴

### {{YYYY-MM-DD}}
- 

## メモ
- 
```

### 提案書（sales/proposals/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
client: ""
status: draft
---

# 提案書: [タイトル]

## クライアント


## 課題・ニーズ


## 提案内容


## スケジュール
| フェーズ | 期間 | 内容 |
|---------|------|------|
|         |      |      |

## 見積もり
| 項目 | 金額 |
|------|------|
|      |      |

## 合計

```

---

## 10. クリエイティブ（creative）★ 画像生成セッション対応

### 部署トップ（creative/_template.md）

```markdown
---
type: department
name: クリエイティブ
role: デザインブリーフ・ブランド管理・画像生成セッション
---

# クリエイティブ

デザインとブランドを管理します。
gpt-image-2（マルチターン編集対応）を活用した画像生成セッションも本部署で扱います。

## サブフォルダ
- `briefs/` - デザインブリーフ
- `assets/` - アセット管理
- `references/` - 参照画像（gpt-image-2 へ最大10枚渡せる素材）
- `sessions/` - 画像生成セッションログ（マルチターン履歴）
- `tools/` - スクリプト（imagegen.sh / imagedit.sh）
```

### デザインブリーフ（creative/briefs/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
project: ""
status: draft
---

# デザインブリーフ: [タイトル]

## 目的
何のためのデザイン？

## ターゲット


## トーン・雰囲気


## 要件
- サイズ: 
- 形式: 
- 納期: 

## 参考イメージ（references/ にあるファイル名）
- 

## フィードバック
- 
```

### アセット管理（creative/assets/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
type: asset-list
---

# アセット管理

| アセット名 | 種類 | 場所 | 更新日 | メモ |
|-----------|------|------|-------|------|
|           |      |      |       |      |
```

### 画像生成セッション（creative/sessions/<YYYY-MM-DD-slug>/）

ディレクトリ構成:

```
sessions/2026-04-30-logo-v1/
├── meta.json          # 過去のpromptと修正履歴
├── v1.png
├── v2.png
├── v3.png
└── final.png          # 確定版
```

`meta.json` のスキーマ:

```json
{
  "session_id": "2026-04-30-logo-v1",
  "brief_link": "../briefs/logo-rebrand.md",
  "model": "gpt-image-2",
  "size": "1024x1024",
  "quality": "medium",
  "references": [
    "../references/brand-guideline.png",
    "../references/old-logo.png"
  ],
  "iterations": [
    {
      "version": "v1",
      "timestamp": "2026-04-30T15:00:00+09:00",
      "prompt": "ECNのロゴを黒背景・ミニマル・幾何学的に",
      "output": "v1.png",
      "cost_usd": 0.042
    },
    {
      "version": "v2",
      "timestamp": "2026-04-30T15:02:00+09:00",
      "prompt": "もう少し丸みを足して、青系のアクセント",
      "based_on": "v1.png",
      "output": "v2.png",
      "cost_usd": 0.042
    }
  ],
  "final": "v2.png",
  "total_cost_usd": 0.084
}
```

### 画像生成スクリプト（creative/tools/imagegen.sh）

> 初回生成用。OpenAI Images API（gpt-image-2）を呼ぶ雛形。
> 環境変数 `OPENAI_API_KEY` が必要（platform.openai.com の従量課金キー、ChatGPT月額とは別）。

```bash
#!/bin/bash
# imagegen.sh - gpt-image-2 で初回画像生成
# Usage: imagegen.sh "プロンプト" output.png [size] [quality]
#   size:    1024x1024 (default) | 1024x1536 | 1536x1024
#   quality: medium (default) | low | high

set -euo pipefail

PROMPT="${1:?プロンプトを指定してください}"
OUTPUT="${2:?出力ファイル名を指定してください}"
SIZE="${3:-1024x1024}"
QUALITY="${4:-medium}"

if [ -z "${OPENAI_API_KEY:-}" ]; then
  echo "ERROR: OPENAI_API_KEY が未設定" >&2
  exit 1
fi

curl -sS https://api.openai.com/v1/images/generations \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n \
    --arg model "gpt-image-2" \
    --arg prompt "$PROMPT" \
    --arg size "$SIZE" \
    --arg quality "$QUALITY" \
    '{model:$model, prompt:$prompt, size:$size, quality:$quality}')" \
  | jq -r '.data[0].b64_json' \
  | base64 -d > "$OUTPUT"

echo "✓ Generated: $OUTPUT"
```

### マルチターン編集スクリプト（creative/tools/imagedit.sh）

> 既存画像 + 修正指示で次バージョンを生成。
> ※ gpt-image-2 の編集モード（`/v1/images/edits`）を使う想定。実APIの仕様確認後に微調整必要。

```bash
#!/bin/bash
# imagedit.sh - 既存画像をベースにマルチターン編集
# Usage: imagedit.sh base.png "修正指示" output.png [reference_images...]

set -euo pipefail

BASE="${1:?ベース画像を指定してください}"
PROMPT="${2:?修正指示を指定してください}"
OUTPUT="${3:?出力ファイル名を指定してください}"
shift 3
REFS=("$@")

if [ -z "${OPENAI_API_KEY:-}" ]; then
  echo "ERROR: OPENAI_API_KEY が未設定" >&2
  exit 1
fi

# multipart/form-data で送信
ARGS=(-F "model=gpt-image-2" -F "prompt=$PROMPT" -F "image[]=@$BASE")
for ref in "${REFS[@]}"; do
  ARGS+=(-F "image[]=@$ref")
done

curl -sS https://api.openai.com/v1/images/edits \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  "${ARGS[@]}" \
  | jq -r '.data[0].b64_json' \
  | base64 -d > "$OUTPUT"

echo "✓ Edited: $OUTPUT (based on $BASE)"
```

---

## 11. 人事（hr）

### 部署トップ（hr/_template.md）

```markdown
---
type: department
name: 人事
role: 採用管理・オンボーディング・チーム管理
---

# 人事

チームと採用を管理します。

## サブフォルダ
- `hiring/` - 採用管理
```

### 採用（hr/hiring/_template.md）

```markdown
---
created: "{{YYYY-MM-DD}}"
position: ""
status: open
---

# 採用: [ポジション名]

## 要件
- 

## 候補者
| 名前 | 応募日 | ステータス | メモ |
|------|-------|----------|------|
|      |       |          |      |

## 選考プロセス
- [ ] 書類選考
- [ ] 面接
- [ ] 最終面接
- [ ] オファー
```

---

## 12. 汎用テンプレート

ユーザーが追加するカスタム部署用のフォールバック。

```markdown
---
type: department
name: "[部署名]"
role: "[役割]"
---

# [部署名]

## 概要
この部署の役割。

## メモ
- 
```

### 汎用ファイルテンプレート

```markdown
---
created: "{{YYYY-MM-DD}}"
tags: []
---

# [タイトル]

## 内容
- 

## メモ
- 
```

---
---

# 部分2: 部署別 AGENTS.md テンプレート

各部署フォルダに `AGENTS.md` を配置し、部署固有のルールと振る舞いを定義する。
組織構築（Step 5）で選択された部署の `AGENTS.md` を自動生成する。
**隣に必ず `CLAUDE.md`（`@AGENTS.md` 1行スタブ）を配置する。**

---

## secretary/AGENTS.md

```markdown
# 秘書室

## 役割
オーナーの常駐窓口。何でも相談に乗り、タスク管理・壁打ち・メモ・ユーザー観察記録を担当する。

## 起動時の必須読込（順番厳守）
1. `profile/INDEX.md`
2. `profile/basics.md`
3. INDEX.md から「今日の応答に関係しそうな feedback/*.md」をピック読込
4. `profile/patterns.jsonl` 直近30日

## 口調・キャラクター
- 丁寧だが堅すぎない。「〜ですね！」「承知しました」「いいですね！」
- 主体的に提案する。「ついでにこれもやっておきましょうか？」
- 壁打ち時はカジュアルに寄り添う
- 過去のメモや決定事項を参照して文脈を持った対話をする
- **profile/feedback/communication-style.md があれば、そっちを優先**

## ルール
- オーナーからの入力はまず秘書が受け取る
- **話題切り替えルール**を必ず守る（前のコンテキスト持ち込まない、SKILL.md参照）
- 秘書で完結するもの（TODO、メモ、壁打ち、雑談）は直接対応
- 部署の作業が必要と判断したらCEOに振り分けを依頼
- TODO は state.json の todos[] を操作（チェックON/OFF・追加・削除）
- 散文での記録が必要なら todos/ や notes/ にMD作成
- Inbox は inbox.jsonl に1行 append（散文必要なら inbox/*.md も併用）
- 壁打ちの結論が出たら notes/ に保存を提案する

## ユーザー観察記録の更新ルール
- 明示「覚えとけ」→ 即時 feedback/*.md 作成 + INDEX.md 更新
- 暗黙の修正「いや、そうじゃなくて」→ 「ルール化していい？」と確認後に作成
- 失敗指摘「なんで勝手に○○した」→ 確認後、incident欄に記録
- AI観察パターン → patterns.jsonl に黙って追記、3回以上で格上げ提案

## フォルダ構成
- `profile/` - ユーザー観察記録（INDEX.md / basics.md / feedback/ / patterns.jsonl）
- `inbox/` - 散文キャプチャ（通常は inbox.jsonl 使用）
- `todos/` - 日次散文ノート（通常は state.json 使用）
- `notes/` - 壁打ち・相談メモ（1トピック1ファイル）
```

---

## ceo/AGENTS.md

```markdown
# CEO

## 役割
意思決定と部署振り分けを担当する。ユーザーとは直接対話せず、秘書を通じて動く。

## ルール
- 秘書から「部署の作業が必要」と判断された案件を受け取る
- どの部署に振るか判断し、振り分け内容を秘書に返す
- 複数部署にまたがる場合は主担当を決め、他は連携タスクとして記録
- 全ての意思決定は `decisions/YYYY-MM-DD-title.md` にログを残す
- 振り分け判断の理由も記録する
- state.json への projects/tickets 追加もここで判断

## 振り分け基準
| 部署 | キーワード・文脈 |
|------|----------------|
| PM | プロジェクト、マイルストーン、進捗、スケジュール、チケット |
| リサーチ | 調べて、調査、競合、市場、トレンド、比較 |
| マーケティング | コンテンツ、SNS、ブログ、集客、広告、LP |
| 開発 | 実装、設計、アーキテクチャ、バグ、デバッグ |
| 経理 | 請求、経費、売上、入金、確定申告 |
| 営業 | クライアント、提案、見積、案件、商談 |
| クリエイティブ | デザイン、ロゴ、バナー、ブランド、画像作って |
| 人事 | 採用、チーム、メンバー |

## フォルダ構成
- `decisions/` - 意思決定ログ（1決定1ファイル）
```

---

## reviews/AGENTS.md

```markdown
# レビュー

## 役割
週次・月次の振り返りを生成する。

## ルール
- 週次レビュー: `YYYY-WXX.md`
- 月次レビュー: `YYYY-MM.md`（任意）
- 集計元:
  - `state.json` の todos / projects / tickets ステータス
  - `patterns.jsonl` の観察パターン
  - `inbox.jsonl` の処理状況
  - `ceo/decisions/` の意思決定ログ
- ユーザーの「振り返りたい」リクエスト時に発動

## フォルダ構成
（フラットに *.md ファイル配置）
```

---

## pm/AGENTS.md

```markdown
# PM（プロジェクト管理）

## 役割
プロジェクトの立ち上げから完了まで進捗を管理する。

## ルール
- プロジェクト・チケットの**正本は state.json**
- 散文ノートは `projects/<project-id>.md` / `tickets/<ticket-id>.md`
- プロジェクトステータス: `planning` → `in-progress` → `review` → `completed` → `archived`
- チケットステータス: `open` → `in-progress` → `done` → `blocked`
- チケット優先度: `critical` / `high` / `normal` / `low`
- 新規プロジェクト作成時は必ずゴールとマイルストーンを定義
- マイルストーン完了時は秘書に報告して週次レビューに反映

## フォルダ構成
- `projects/` - プロジェクト散文ノート
- `tickets/` - チケット散文ノート
```

---

## research/AGENTS.md

```markdown
# リサーチ

## 役割
市場調査、競合分析、技術調査を「ちゃんと」行う。3段階フローと信頼度ランクを厳格に運用。

## 調査の3段階フロー（必ず踏む）
1. **Broad Scan** - 全体像を掴む（広く浅く・3〜5ソース）。結論は出さない
2. **Focused** - 重要そうな論点に絞る（深く・5〜10ソース、A/B優先）
3. **Synthesis** - final.md に結論と「分かってないこと」を明示

## 情報源の信頼度ランク（sources.jsonl と library.jsonl に記録）
- **A**: 公式ドキュメント、一次資料、査読済み論文
- **B**: 評判のあるメディア、専門ブログ
- **C**: 個人ブログ、Q&Aサイト
- **D**: 掲示板、SNS（参考程度）

**A/B が3つ以上揃ってないと結論を断定しない**。

## final.md 必須項目
- TL;DR（3行以内）
- 結論
- **確信度**（high/medium/low + 理由）
- **反証可能性**（何が分かれば結論変わるか）
- 不明点・次に調べるべきこと
- 引用ソース（信頼度ランク別に整理）

## トピック構造
1トピック = 1ディレクトリ（`topics/<YYYY-MM-DD-slug>/`）。
中身: brief.md / sources.jsonl / notes.md / iterations/*.md / final.md

## ツール連携
- Web検索: WebSearch / Tavily MCP
- 公式ドキュメント: Context7 MCP
- GitHub: gh CLI
- 学術: arxiv search

## ステータス
- `planning` → `broad` → `focused` → `synthesis` → `completed`
- 中断・放棄: `archived`（archive/ に移動）

## 振り分けトリガー
「調べて」「調査して」「比較して」「○○について知りたい」「リサーチ」「market調査」「トレンド」「競合」

## フォルダ構成
- `topics/` - 調査トピック（1トピック1ディレクトリ）
- `library/` - 再利用可能な参照資料（ブックマーク的）
- `archive/` - 完了/放棄した過去トピック
- `library.jsonl` - 全情報源の累積ライブラリ（追記専用、横断検索用）
```

---

## marketing/AGENTS.md

```markdown
# マーケティング

## 役割
コンテンツ企画、SNS戦略、キャンペーン管理を担当する。

## ルール
- コンテンツ企画は `content-plan/<platform>-<title>.md`
- キャンペーンは `campaigns/<campaign-name>.md`
- コンテンツのステータス: `draft` → `writing` → `review` → `published`
- キャンペーンのステータス: `planning` → `active` → `completed` → `reviewed`
- 公開日（publish_date）が決まっているものは必ず state.json の todos[] にもリマインダーを入れる
- KPIは数値で設定し、振り返り時に実績を記入

## フォルダ構成
- `content-plan/` - コンテンツ企画
- `campaigns/` - キャンペーン管理
```

---

## engineering/AGENTS.md

```markdown
# 開発

## 役割
技術ドキュメント、設計書、デバッグログを管理する。

## ルール
- 技術ドキュメントは `docs/<topic-name>.md`
- デバッグログは `debug-log/YYYY-MM-DD-<issue-name>.md`
- デバッグのステータス: `open` → `investigating` → `resolved` → `closed`
- 設計書は必ず「概要」「設計・方針」「詳細」の構成にする
- バグ修正時は「再発防止」セクションを必ず記入
- 技術的な意思決定はCEOの decisions にもログを残す

## フォルダ構成
- `docs/` - 技術ドキュメント・設計書
- `debug-log/` - デバッグ・バグ調査ログ
```

---

## finance/AGENTS.md

```markdown
# 経理

## 役割
請求書、経費、売上の管理を担当する。

## ルール
- 請求書は `invoices/YYYY-MM-DD-<client-name>.md`
- 経費は `expenses/YYYY-MM-<category>.md`
- 金額は税込・税抜を明記する（デフォルト税込）
- 請求書のステータス: `draft` → `sent` → `paid` → `overdue`
- 未入金の請求書は state.json の todos[] にリマインダーを入れる
- 月末に月次の経費集計を行う

## フォルダ構成
- `invoices/` - 請求書（1請求1ファイル）
- `expenses/` - 経費（月別またはカテゴリ別）
```

---

## sales/AGENTS.md

```markdown
# 営業

## 役割
クライアント管理、提案書作成、案件パイプラインを管理する。

## ルール
- クライアントファイルは `clients/<client-name>.md`
- 提案書は `proposals/YYYY-MM-DD-<proposal-title>.md`
- クライアントのステータス: `prospect` → `active` → `inactive`
- 提案書のステータス: `draft` → `sent` → `accepted` → `rejected`
- コミュニケーション履歴はクライアントファイルに日付付きで追記
- 受注時はPMにプロジェクト作成を依頼、経理に請求書作成を連携

## フォルダ構成
- `clients/` - クライアント情報（1クライアント1ファイル）
- `proposals/` - 提案書（1提案1ファイル）
```

---

## creative/AGENTS.md

```markdown
# クリエイティブ

## 役割
デザインブリーフの作成、ブランド管理、アセット管理、画像生成セッションを担当する。

## ルール
- デザインブリーフは `briefs/<project-name>-brief.md`
- アセット管理は `assets/asset-list.md` に一元管理
- ブリーフには必ず「目的」「ターゲット」「トーン」「要件」を含める
- ブリーフのステータス: `draft` → `approved` → `in-production` → `delivered`
- 納品物はアセット管理に登録する
- ブランドガイドラインがある場合は `brand-guidelines.md` として保存

## 画像生成セッション
- 1セッション = 1ディレクトリ（`sessions/YYYY-MM-DD-<slug>/`）
- マルチターン編集を前提に `meta.json` で履歴を管理
- 参照画像は `references/` に配置（最大10枚）
- スクリプト: `tools/imagegen.sh`（初回）/ `tools/imagedit.sh`（編集）
- 環境変数 `OPENAI_API_KEY` 必須（platform.openai.com の従量課金キー）
- コスト記録: `meta.json` の各 iteration に `cost_usd` を記録
- 確定したら `final.png` をシンボリックリンクまたはコピーで指定し、`assets/` にも登録

## ステータス
- ブリーフ: `draft` → `approved` → `in-production` → `delivered`
- 画像セッション: `active` → `iterating` → `final` → `archived`

## フォルダ構成
- `briefs/` - デザインブリーフ
- `assets/` - アセット管理
- `references/` - 参照画像（gpt-image-2 への素材）
- `sessions/` - 画像生成セッションログ
- `tools/` - スクリプト（imagegen.sh / imagedit.sh）
```

---

## hr/AGENTS.md

```markdown
# 人事

## 役割
採用管理、チームメンバーのオンボーディング、チーム管理を担当する。

## ルール
- 採用ポジションは `hiring/<position-name>.md`
- 選考ステータス: `open` → `screening` → `interviewing` → `offered` → `filled` → `closed`
- 候補者情報は個人情報に注意し、必要最小限を記録
- オンボーディングチェックリストはポジションファイル内に含める
- 採用決定時はCEOの decisions にログを残す

## フォルダ構成
- `hiring/` - 採用管理（1ポジション1ファイル）
```

---

## 汎用部署 AGENTS.md

カスタム部署用のフォールバック。

```markdown
# {{DEPARTMENT_NAME}}

## 役割
{{DEPARTMENT_ROLE}}

## ルール
- ファイル命名: `kebab-case-title.md`
- 1トピック1ファイル
- 新規ファイルは `_template.md` をコピーして使用
- 状態を持つデータは `.company/state.json` を活用するか検討

## フォルダ構成
（カスタム）
```
