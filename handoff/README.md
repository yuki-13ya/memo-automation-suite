# handoff

このフォルダは、`memo-workflow` と `ticktick-task` の間で共有する申し送りを置く場所です。

## 役割

- `memo-workflow` 側から `ticktick-task` 側へ渡す仕様、入力、出力、確認事項の申し送り
- `ticktick-task` 側から `memo-workflow` 側へ戻す修正依頼や受け入れ条件
- フォルダ移動、受け渡し確認、移行メモなど、どちらか一方のリポジトリに寄せにくい連絡事項

## 書くタイミング

以下の場合は、このフォルダに申し送りを残します。

- `memo-workflow` の出力仕様を `ticktick-task` 側で受け入れる前に、項目差分や不足項目を整理するとき
- `ticktick-task` 側で必要になった入力項目を、`memo-workflow` 側へ修正依頼するとき
- 両プロジェクトにまたがる判断を、片方のdocsだけに閉じ込めると後で追いにくいとき
- フォルダ移動、親プロジェクト化、将来の分離候補など、構造上の移行メモを残すとき

## 文書の位置づけ

このフォルダの文書は、原則として申し送りや合意メモです。

固定仕様の正本は、内容に応じて `memo-workflow` または `ticktick-task` のdocsに置きます。

このフォルダに書いた内容が確定仕様になった場合は、該当する子プロジェクト側の正本文書へ反映する必要があるか確認します。

`ticktick_list_names.json` は、`memo-workflow` の文脈ラベル辞書から `ticktick-task` のレビューUIへ渡すTickTickリスト名候補です。登録先リストの正本候補として使い、候補外の反映先はレビューUI側で受信箱扱いにします。

## 置かないもの

- 認証情報、トークン、`.env` の実値
- TickTick APIの実行結果や個人情報を含む実データ
- どちらかのリポジトリで正本管理すべき実装ファイル

## 命名方針

申し送り文書を追加する場合は、内容が分かる短い英語名または日本語ローマ字名を使います。

例:

```text
memo_workflow_phase5_to_ticktick_intake.md
ticktick_intake_schema_feedback.md
notion_route_future_split_note.md
```

一時的な作業メモは、正式な申し送りとして残す必要があるか確認してから追加します。

## 責務の分け方

`memo-automation-suite` 全体の開発方針、Phase構成、上流ワークフローの整理は、原則として `memo-workflow` 側で扱います。

TickTickタスクの登録、統合、レビュー、分解、差し戻し、作業ブロック分類など、タスク関連に特化した設計と実装は、`ticktick-task` 側で扱います。

このフォルダは、両者の受け渡しを中立に残すための共有置き場です。
