# update-006: fmcs-utils の AGENTS.md によるレビュー定型指示の永続化をテンプレートへ取り込み

## ステータス

Closed（2026-08-27 起票・完了）

## 目的・背景

fmcs-utils の update-006（AGENTS.md によるレビュー定型指示の永続化）で確立した運用を、テンプレートへ取り込む。

Codex へのレビュー依頼文には、毎回同じ定型指示（瑣末な指摘の抑止・重要度(高/中/低)分類・修正提案の要求）を含めている。fmcs-utils では、この定型指示をリポジトリ直下の `AGENTS.md`（Codex CLI が起動時に自動で読み込む指示ファイル）へ移し、依頼文を「対象ファイル＋案件種別ごとの基準・観点の指定」だけに短縮した。あわせて、読み込み確認用マーカー「[AGENTS.md適用]」を回答冒頭に表示させ、依頼側が読み込みの成否を検出できるようにした（読み込み失敗時に素の挙動へ戻っても気づけない「静かな欠落」への対策）。

現行テンプレートには `AGENTS.md` が存在せず、`template/CLAUDE.md` の4つのコマンド例（初回レビュー3種＋再レビュー1種）すべてに定型指示が埋め込まれている。

## 反映元

- リポジトリ: `/home/staff/sakagawa/git/fmcs-utils`（コミット `8868925ee3ffd351d6456ebf83871401e0712740`、2026-08-27 時点）
- 案件: `docs/issues/update-006-codex-agents-md/`（README.md / design.md）
- 反映後の実ファイル: リポジトリ直下の `AGENTS.md`、`CLAUDE.md` のレビュー節（依頼文の基準部分・依頼の送り方・設定段落）

## 調査結果

### fmcs-utils 側で検証済みの事実（fmcs-utils update-006 README.md より）

- Codex CLI は `~/.codex/AGENTS.md`（グローバル）と、Git ルートから作業ディレクトリまで各階層の `AGENTS.md` を起動時に読み込む（結合上限 32 KiB）。ディレクトリ信頼状態に関係なく読まれる
- 指示チェーンは**セッション開始時に一度だけ構築**される。ファイル変更は既存セッションに反映されない
- 実機検証（E1/E2）で、定型文なしの素の依頼でも AGENTS.md の指示（瑣末指摘の抑止・重要度分類・修正提案・適用マーカー）が守られることを確認済み
- グローバル `~/.codex/AGENTS.md` は他プロジェクトに影響するため使わず、リポジトリ直下に置く

### テンプレート側の現状

- `AGENTS.md` に相当するファイルは `template/` 配下に存在しない
- `template/CLAUDE.md` の「Codexによるレビューの実行方法」の4つのコマンド例すべてに定型指示（「瑣末な点へのクソリプはしないで、致命的な点のみ指摘して。発見した問題を重要度(高/中/低)で分類し、修正提案とともに報告すること。」等）が埋め込まれている
- ルート `CLAUDE.md` の「テンプレートの使い方」手順1（コピー対象一覧）とディレクトリ構成に `AGENTS.md` の記載がない

### 反映方式の重要な相違（fmcs-utils との差）

fmcs-utils では本変更を **Herdr 対話方式**（fmcs-utils update-004 反映後）のレビュー節に適用したが、テンプレートは現時点で **`codex exec` + `resume` 方式**のままである（Herdr 方式の取り込みは本リポジトリの update-004 案件として本案件の後に行う予定）。よって本案件では fmcs-utils の変更を現行方式に読み替えて適用する:

| fmcs-utils 側 | 本案件での読み替え |
|---|---|
| Herdr 用依頼文テンプレートの短縮 | `codex exec` コマンド例4つの依頼文の短縮 |
| `agent read` での結果回収時にマーカー確認 | `codex-NN.result.md` を読む際に冒頭のマーカー確認 |
| 「AGENTS.md 変更時は稼働中のレビューストリームを再起動」 | 「AGENTS.md の変更は `resume` で継続中のセッションには反映されない（新規の `codex exec` から有効）」 |

本リポジトリの update-004 案件でレビュー節を Herdr 方式へ差し替える際は、fmcs-utils の最終版（本変更を統合済み）を反映元とするため、本案件の読み替え部分はそこで上書きされる。`AGENTS.md` 本体とディレクトリ構成・コピー手順の変更はそのまま残る。

## 反映対象の選別

| fmcs-utils 側の変更 | 取り込み | 理由 |
|---|---|---|
| リポジトリ直下 `AGENTS.md` の新規作成 | ○ | 本文はプロジェクト固有記述ゼロ。改変なしで `template/AGENTS.md` として転用できる |
| CLAUDE.md 依頼文テンプレートの短縮 | ○（読み替え） | 上記のとおり現行の `codex exec` コマンド例に適応して取り込む |
| 回収時の適用マーカー確認 | ○（読み替え） | `result.md` 冒頭の確認に読み替え |
| AGENTS.md 変更時の再起動注意 | ○（読み替え） | `resume` セッションへの非反映の注意に読み替え |
| V4 実機確認（Herdr ストリームでのマーカー検証） | ○（読み替え） | Herdr ストリームでの検証は fmcs-utils 固有のため、テンプレート側では「コピー先相当の一時リポジトリで定型指示なしの `codex exec` を1回実行し、適用マーカーを確認する」実機確認（design.md 4章 検証5）に読み替えて取り込む |
| fmcs-utils の BACKLOG / CHANGELOG / 案件 README の完了処理 | × | fmcs-utils 固有の案件管理記録 |

## スコープ

### 変更対象

- `template/AGENTS.md`: 新規作成（fmcs-utils の `AGENTS.md` と同一内容）
- `template/CLAUDE.md`: 依頼文の短縮（4箇所）、定型指示の供給元とマーカー確認の明記、ディレクトリ構成への `AGENTS.md` 追加
- 完了処理: ルート `CLAUDE.md`（ディレクトリ構成・「テンプレートの使い方」手順1）、本案件 README

### スコープ外

- レビュー方式の Herdr 化（本リポジトリの update-004 案件で扱う）
- 本リポジトリ（DEV_TEMPLATE）ルートへの `AGENTS.md` 配置（テンプレート payload の整備が本案件の目的。本リポジトリ自身のレビュー運用の変更は必要になった時点で別途検討する）
- fmcs-utils 側のファイルの変更（一切行わない）

## 反映時の実機確認結果（2026-08-27・design.md 4章 検証5）

合格。`~/tmp` 配下の一時ディレクトリへ `template/` 一式（`.gitignore` 含む）をコピーして `git init` し、定型指示を含まない依頼（`docs/BACKLOG.md をレビューしてください。`）を `codex exec --cd`（codex-cli 0.150.1）で1回実行した。回答冒頭に「[AGENTS.md適用]」が表示され、瑣末な指摘もなかった（本文は「致命的な問題は見つかりませんでした。」のみ）。確認後、一時ディレクトリは削除済み。

## 関連ファイル

- 反映元: `/home/staff/sakagawa/git/fmcs-utils/docs/issues/update-006-codex-agents-md/`、`/home/staff/sakagawa/git/fmcs-utils/AGENTS.md`
- 反映先: `template/AGENTS.md`（新規）、`template/CLAUDE.md`
- 反映設計書: 本案件フォルダの `design.md`
