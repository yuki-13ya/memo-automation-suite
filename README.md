# memo-automation-suite

`memo-automation-suite` は、Discordメモから記録化とタスク化へ進む一連の自動化を束ねる親プロジェクトです。

この親フォルダは、実装を集中させる場所ではなく、`memo-workflow` と `ticktick-task` の責務分担、受け渡し、横断的な運用方針を確認する入口として扱います。

## フォルダ構成

| フォルダ | 役割 |
|---|---|
| `memo-workflow/` | Discordメモを取得し、原文を維持したまま分類し、NotionルートとTODO候補ルートへ分岐させる上流プロジェクト |
| `ticktick-task/` | TODO候補を受け取り、既存TickTickタスクとの統合判定、人間確認、反映、作業ブロック化を扱う下流プロジェクト |
| `handoff/` | 2つのプロジェクト間で共有する申し送り、受け入れ条件、修正依頼、移行メモの置き場 |

## 基本方針

親フォルダでは、次の内容を扱います。

- どの作業をどの子プロジェクトで扱うかの判断
- `memo-workflow` と `ticktick-task` の受け渡し境界
- 片方の仕様変更がもう片方へ影響する場合の申し送り
- 将来、プロジェクトを分けるかどうかの判断材料

親フォルダでは、次の内容を原則として扱いません。

- 各子プロジェクトの実装本体
- 個別Phaseの詳細仕様
- API認証情報、トークン、`.env` の実値
- 実データや個人情報を含む出力

## 責務分担

`memo-workflow` は、Discordメモを起点にした上流ワークフローを扱います。

- Discord投稿の取得
- 原文維持のトピック分類
- Notion向け記録データの生成
- Notion API転記の検討と実装
- `ticktick-task` 向けTODO候補JSON生成

`ticktick-task` は、TickTick側のタスク処理を扱います。

- TODO候補JSONの受け取り
- 既存TickTickタスクとの統合判定
- 人間確認用データの生成
- OK済み候補のTickTick反映
- 未完了タスクの作業ブロック分類
- 作業ブロック用タスクのTickTick出力
- Googleカレンダーの空き時間参照に関係するTickTick側処理

`handoff` は、どちらか片方に寄せにくい受け渡し情報を扱います。

- 入出力仕様の差分確認
- 下流から上流への修正依頼
- 上流から下流への申し送り
- フォルダ移動や統合運用に関する移行メモ

## Notionルートの扱い

Notion向けMarkdown生成やNotion API転記は、現時点では `memo-workflow` 内のNotionルートとして扱います。

Notion関連の処理が、Discordメモ以外の入力、複数DB同期、差分同期、再実行管理、専用レビューUIなどを持つ独立した基盤になった場合は、将来別プロジェクトとして分離することを検討します。

## 作業時の読み順

親フォルダで作業するときは、まずこのREADMEを確認します。

次に、作業内容に応じて以下を確認します。

| 作業内容 | 読み先 |
|---|---|
| 親フォルダ全体の作業ルール | `AGENTS.md` |
| `memo-workflow` 側の作業 | `memo-workflow/README.md`, `memo-workflow/AGENTS.md`, `memo-workflow/docs/document-index.md` |
| `ticktick-task` 側の作業 | `ticktick-task/README.md`, `ticktick-task/AGENTS.md`, `ticktick-task/docs/document-index.md` |
| 2プロジェクト間の申し送り | `handoff/README.md` |

## 運用ルール

実装変更は、原則として該当する子プロジェクト内で行います。

両方の子プロジェクトにまたがる変更は、先に責務分担と受け渡し仕様を確認し、必要に応じて `handoff/` に申し送りを残します。

親フォルダは、片方の子プロジェクトの都合だけで共有仕様を書き換える場所にはしません。共有仕様を変える場合は、影響する子プロジェクト側の正本文書も確認します。
