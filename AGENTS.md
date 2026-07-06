# AGENTS.md

このファイルは、Codexが `memo-automation-suite` 親フォルダで作業するときに守るルールをまとめたものです。

## 1. 基本方針

`memo-automation-suite` は、`memo-workflow` と `ticktick-task` を束ねる親プロジェクトです。

親フォルダは、実装本体を集める場所ではなく、責務分担、読み順、受け渡し、横断的な運用方針を整理する場所として扱います。

作業前に、どのフォルダの責務に属する変更かを確認します。

## 2. フォルダごとの責務

`memo-workflow` は、Discordメモを起点にした上流ワークフローを扱います。

- Discord投稿の取得
- 原文維持のトピック分類
- Notion向け記録データの生成
- Notion API転記
- `ticktick-task` 向けTODO候補JSON生成

`ticktick-task` は、TickTick側のタスク処理を扱います。

- TODO候補JSONの受け取り
- 既存TickTickタスクとの統合判定
- 人間確認
- OK済み候補のTickTick反映
- TickTick未完了タスクの作業ブロック分類
- 作業ブロック用タスクのTickTick出力
- Googleカレンダー空き時間参照に関係するTickTick側処理

`handoff` は、2つの子プロジェクトの間で共有する申し送りを扱います。

- 上流から下流への入力仕様、出力仕様、確認事項
- 下流から上流への修正依頼、受け入れ条件
- どちらか一方のリポジトリに寄せにくい移行メモ

## 3. 作業対象の判断

Discord取得、分類、Notionルート、TODO候補JSON生成に関する変更は、原則として `memo-workflow` で扱います。

TickTick登録、更新、統合判定、人間確認、作業ブロック化、TickTick側のGoogleカレンダー空き時間参照に関する変更は、原則として `ticktick-task` で扱います。

両方にまたがる仕様や受け渡し条件は、まず各子プロジェクトの正本文書を確認し、必要に応じて `handoff` に申し送りを残します。

親フォルダには、子プロジェクト内で完結する実装、Phase詳細仕様、個別の動作ログを置きません。

## 4. Notionルートの扱い

Notion向けMarkdown生成やNotion API転記は、現時点では `memo-workflow` のNotionルートとして扱います。

Notion関連処理が、Discordメモ以外の入力、複数DB同期、差分同期、再実行管理、専用レビューUIなどを持つ独立した基盤になった場合は、別プロジェクト化を検討します。

別プロジェクト化するまでは、親フォルダ側では「将来分離候補」として扱い、実装や詳細仕様は `memo-workflow` 側へ置きます。

## 5. 作業前に確認する文書

親フォルダで作業するときは、必要に応じて以下を確認します。

- `README.md`
- `AGENTS.md`
- `handoff/README.md`
- `memo-workflow/README.md`
- `memo-workflow/AGENTS.md`
- `memo-workflow/docs/document-index.md`
- `ticktick-task/README.md`
- `ticktick-task/AGENTS.md`
- `ticktick-task/docs/document-index.md`

毎回すべての文書を全文で深掘りするのではなく、作業の影響範囲に応じて読む文書を選びます。

実装変更に進む場合は、対象子プロジェクトの `AGENTS.md` と `docs/document-index.md` を確認します。

## 6. 文書の扱い

親 `README.md` は、全体の入口、責務分担、読み順を示す文書として扱います。

親 `AGENTS.md` は、親フォルダで作業するときのルール、子プロジェクト間の境界、`handoff` の使い方を示します。

`handoff/README.md` は、申し送り文書の置き方、書く内容、書かない内容を示します。

個別Phaseの仕様、詳細な実装方針、確認状況、変更履歴は、該当する子プロジェクト側のdocsへ置きます。

未解決事項は、片方の子プロジェクトだけで閉じる場合はその子プロジェクトの `docs/issues.md` に置きます。両方にまたがる場合は、`handoff` に申し送りを残したうえで、各子プロジェクト側の正本文書へ反映する必要があるか確認します。

## 7. 実装の進め方

親フォルダでは、実装を追加する前に、その実装が本当に親に属するか確認します。

実装が `memo-workflow` または `ticktick-task` の責務に属する場合は、親ではなく子プロジェクト内で変更します。

両方の子プロジェクトを同時に変更する場合は、以下を先に確認します。

- 変更の起点は上流か下流か
- 正本となる仕様書はどちらにあるか
- 受け渡しファイルの入力、出力、必須項目が一致しているか
- 片方だけを変更しても破綻しないか
- `handoff` に申し送りを残す必要があるか

## 8. 外部連携と秘密情報

親フォルダには、認証情報、トークン、`.env` の実値、APIレスポンスの実データ、個人情報を置きません。

外部サービスへの読み取り、書き込み、AI利用、DryRun / Confirm / Execute の扱いは、該当する子プロジェクトのルールに従います。

`memo-workflow` 側では、TickTick APIへの登録、更新、削除を扱いません。

`ticktick-task` 側では、Discordメモ取得やNotion転記を扱いません。

## 9. Git運用

親フォルダ、`memo-workflow`、`ticktick-task` は、それぞれ独立したGitリポジトリとして扱います。

GitHubの対応先は以下です。

| ローカルフォルダ | GitHubリポジトリ |
|---|---|
| `memo-automation-suite/` | `https://github.com/yuki-13ya/memo-automation-suite` |
| `memo-workflow/` | `https://github.com/yuki-13ya/memo-workflow` |
| `ticktick-task/` | `https://github.com/yuki-13ya/ticktick-task` |

親リポジトリでは、子リポジトリ本体を追跡しません。`memo-workflow/` と `ticktick-task/` は親の `.gitignore` で除外します。

子プロジェクト内で作業する場合は、それぞれのGit運用ルールに従います。

作業前に、対象子プロジェクトの差分を確認します。

子プロジェクトの未コミット変更を、親フォルダ作業のついでに巻き戻したり整理したりしません。

コミットまたはpushの前に、以下を確認します。

- `git status` で対象リポジトリと変更ファイルを確認する。
- `git diff` または `git diff --cached` で、コミット対象の内容を確認する。
- `.env`、token、APIキー、認証ファイル、認証キャッシュが追跡対象に入っていないことを確認する。
- Discord実ログ、Notion/TickTick/Google系の実データ、個人情報、クライアント情報、実案件名入りの設定ファイルやサンプルが追跡対象に入っていないことを確認する。
- `logs/`、`outputs/`、`exports/`、`tmp/`、`cache/`、`.cache/` などの実行時生成物が、必要に応じて `.gitignore` に入っていることを確認する。
- 現在ブランチ名と `remote origin` が、意図したGitHubリポジトリを向いていることを確認する。

実データの構造を共有したい場合は、実ファイルをGit管理せず、匿名化した `.example` ファイルを追跡します。

代表例:

| ローカル実データ | Git管理する匿名例 |
|---|---|
| `memo-workflow/config/context_aliases.csv` | `memo-workflow/config/context_aliases.example.csv` |
| `handoff/ticktick_list_names.json` | `handoff/ticktick_list_names.example.json` |
| `ticktick-task/samples/*.txt` の実案件サンプル | `ticktick-task/samples/*.example.txt` |

## 10. 作業後の報告

作業後は、以下を分けて報告します。

- 作成・変更したファイル
- 各ファイルの役割
- 今回決めた責務分担
- 今回作成していない範囲
- 確認できたこと
- 未確認のこと
- コミット有無と理由
