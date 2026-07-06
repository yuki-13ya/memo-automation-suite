# memo-workflow Phase 5 to ticktick-task intake handoff

この文書は、`ticktick-task` 側で定義したTODO候補入力ルールを、`memo-workflow` 側のPhase 5出力へ反映するための親フォルダ側申し送りです。

詳細仕様の正本ではなく、両プロジェクトをまたぐ作業の入口として扱います。

## 目的

`memo-workflow` Phase 5のTODO候補JSONを、`ticktick-task` 側の共通入力スキーマへ合わせます。

これにより、`ticktick-task` 側で以下を同じレビュー構造に流せるようにします。

- `memo-workflow` 由来TODO候補レビュー
- 既存TickTick未完了タスクの棚卸レビュー
- 既存TickTickタスクとの統合判定レビュー
- 作業ブロック分類レビュー
- 将来の専用レビュー画面

## 正本となる文書

TickTick側の入力仕様は、以下を正本とします。

```text
ticktick-task/docs/todo_candidate_intake_schema.md
ticktick-task/docs/review_item_schema.md
ticktick-task/docs/handoff_memo_workflow_phase5_intake_schema.md
```

`memo-workflow` 側のPhase 5仕様は、以下を更新対象とします。

```text
memo-workflow/docs/phase5_todo_candidate_json_spec.md
memo-workflow/docs/phase5_ticktick_task_handoff.md
memo-workflow/phase5-todo-candidates/export_todo_candidates.py
```

## 維持する境界

`memo-workflow` 側では、以下の方針を維持します。

- TickTickへ登録、更新、削除しない
- 既存TickTickタスクとの統合判定をしない
- 作業ブロック正式分類をしない
- 人間レビュー後の確定判断をしない
- 元投稿ID、投稿時刻、根拠本文を保持する
- `blockingHint` は正式分類ではなく参考情報として渡す

`ticktick-task` 側では、以下を扱います。

- 既存TickTickタスクとの照合
- 統合判定
- 人間確認CSVまたはレビュー画面
- 承認、却下、編集、分解、保留
- 必要に応じた作業ブロック分類
- 人間確認後のTickTick反映

## 主なすり合わせ項目

`memo-workflow` 側の既存JSON仕様を、以下の方向で `ticktick-task` 入力スキーマへ寄せます。

| memo-workflow側 | ticktick-task側 |
|---|---|
| `candidates` | `items` |
| `candidateId` | `sourceId` |
| `source` | `sourceContext.originalSource` |
| `topicTitle` | `sourceTitle`, `sourceContext.topicTitle` |
| `status` | `initialStatus` |
| `title` | `proposedTitle` |
| `description` | `proposedDescription` |
| `evidenceText` | `sourceText` |
| `sourcePostIds` / `sourcePostTimes` | `sourceRefs` |
| `humanReviewRequired` | `needsReview` |
| `notes` | `sourceContext.notes` |

`humanDecision` と `humanMemo` は、外部入力時点では空文字で付与します。

## memo-workflow側で出さない項目

以下は `memo-workflow` Phase 5の出力JSONには含めません。

```text
matchType
proposedAction
matchedExistingTasks
existingTitleToRename
schedulingStatus
classification
blockingStatus
estimatedMinutesCandidate
estimateConfidence
taskId
projectId
```

これらは、`ticktick-task` 側で既存TickTickタスクや作業ブロック分類と照合してから決めます。

## 次の作業

`memo-workflow` 側で、以下の順にすり合わせます。

1. `docs/phase5_todo_candidate_json_spec.md` をTickTick側入力スキーマへ合わせる。
2. `docs/phase5_ticktick_task_handoff.md` の用語と項目名を更新する。
3. `phase5-todo-candidates/export_todo_candidates.py` の出力JSONを `items` 構造へ変更する。
4. 既存のPhase 5出力サンプルや確認用Markdownがあれば、新しい項目名に合わせる。
5. テストまたはDryRunで、JSONが `ticktick-task/docs/todo_candidate_intake_schema.md` の必須項目を満たすことを確認する。

## この文書で確定しないこと

以下はこの親フォルダの申し送りでは確定しません。

- `ticktick-task` 側のレビューCSV生成処理
- 専用レビュー画面
- OK済み候補のTickTick反映処理
- 要分解AI相談フロー
- Notionルートの仕様

これらは、該当する子プロジェクト側のdocsで扱います。
