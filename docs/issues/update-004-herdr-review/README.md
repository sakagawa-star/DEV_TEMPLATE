# update-004: fmcs-utils の Codexレビュー Herdr 対話方式をテンプレートへ取り込み

## ステータス

Closed（2026-08-27 起票・完了）

## 目的・背景

fmcs-utils の update-004（Codexレビュー運用の Herdr 対話方式への全面移行）で確立した運用を、テンプレートへ取り込む。

fmcs-utils では、`codex exec` + `resume` によるバッチ方式のレビュー運用が、手順の正しさを Claude Code の記憶と自制に依存する構造（スナップショット管理・diff 生成・session id 追跡等）を持ち、運用ミスが繰り返し発生した（fmcs-utils update-003 の2レーン方式は初適用で破綻）。この反省から、Herdr（コーディングエージェント用ターミナルマルチプレクサ、https://herdr.dev/）の隣接ペインで対話モードの codex を常駐させ、Claude Code が `herdr` CLI 経由でレビューを依頼する方式へ全面移行した。失敗しやすい工程（スナップショット・diff・session id 管理）を構造的に除去し、ストリーム単位の並列化を可能にする設計で、fmcs-utils では起票前の実機検証（起動・回収・文脈保持・解消判定・並列・ダイアログ誤爆）とドッグフーディング（本方式でのレビュー4回収束）を経て運用に入っている。

現行テンプレートの `template/CLAUDE.md` は旧方式（`codex exec` + `resume`）のままであり、fmcs-utils で破綻が判明した構造を新規プロジェクトへ配布し続けることになる。

## 反映元

- リポジトリ: `/home/staff/sakagawa/git/fmcs-utils`（コミット `8868925ee3ffd351d6456ebf83871401e0712740`、2026-08-27 時点）
- 案件: `docs/issues/update-004-herdr-review/`（README.md / design.md）
- 反映後の実ファイル: `CLAUDE.md` の「Codexによるレビューの実行方法（Herdr 対話方式）」節・「行き詰まり検出」節ほか、`docs/HERDR_SETUP.md`

**反映元は fmcs-utils の設計書（update-004 design.md）の記載ではなく、現在の `CLAUDE.md` 実ファイル（最終形）とする。** 理由: fmcs-utils では update-004 の後に update-006（AGENTS.md による定型指示の永続化）がレビュー節へ追記されており、本リポジトリは update-006 を先に取り込み済み（本リポジトリの update-006 案件）のため、AGENTS.md 統合済みの最終形を転記するのが整合する。

## Herdr の理解とセットアップ手順

- Herdr はペイン内のエージェント（claude / codex 等）の生存を管理し、状態を `idle` / `working` / `blocked` / `done` / `unknown` に分類して検出する。Claude Code は `herdr` スキル経由で、隣のペインの codex にプロンプト送信（`agent prompt`）・出力読み取り（`agent read`）・状態待機（`agent wait`）ができる
- セットアップ手順は本案件フォルダの `HERDR_SETUP.md`（2026-08-26 Linux 実機検証済み。fmcs-utils の `docs/HERDR_SETUP.md` と同一内容であることを diff で確認済み）。環境セットアップのみを扱いプロジェクト固有記述を含まないため、改変なしで `template/docs/` へ同梱できる

## 調査結果

### fmcs-utils 側の最終形（取り込み対象の構成要素）

1. **「Codexによるレビューの実行方法（Herdr 対話方式）」節**（旧方式節の全面差し替え）: 前提環境（Herdr + codex 必須・フォールバックなし）、モデル設定と AGENTS.md 再読み込みの注意、レビューストリーム（命名・排他）、エージェントのライフサイクル（生存確認→起動→画面確認→終了）、依頼の送り方（`--wait` + `run_in_background`・マーカー確認）、レビューの進め方（反復→`/new` 全文ゲート→収束）、依頼文の基準部分（AGENTS.md 前提の短縮形）、結果の保存（result.md のみ。full.log 廃止）、サブエージェントへの委任（並列レビュー時）、bwrap 注記
2. **「行き詰まり検出（全作業共通・必須）」節**: 同じ指摘の再発・同じ原因での失敗が2回続いたら想定自体を疑い方針を再検討する。「Claude Code 運用ルール」直下に配置
3. **`docs/HERDR_SETUP.md`**: セットアップ手順
4. **周辺の整合修正**: 案件ディレクトリ構成の `reviews/` 行（full.log 言及の削除）、「実装の実行方法」の「委任しない作業」（レビュー定型作業の委任規定への参照）、ドキュメント更新フロー ステップ4（旧方式の重複記載をレビュー節参照に一本化）、Bash ルールの allowlist 例（`Bash(codex exec *)` → `Bash(herdr *)`）

### テンプレート側の現状（旧方式の記述箇所）

- `template/CLAUDE.md`「Codexによるレビューの実行方法」節: `codex exec` コマンド例・`-o` + full.log 分離・`resume` 再レビュー・「Subagent は使わない」規定（update-006 で依頼文短縮とマーカー確認は反映済み）
- `template/CLAUDE.md` の周辺参照: ドキュメント更新フロー ステップ4（バックグラウンド実行・`-o` + full.log 分離・`resume` の列挙）、案件ディレクトリ構成の `reviews/` 行（full.log は gitignore）、「委任しない作業」（Codexレビューの実行と指摘反映）、Bash ルールの allowlist 例 `Bash(codex exec *)`
- `template/.gitignore`: `docs/issues/*/reviews/*.full.log` の除外設定
- 機能追加・不具合修正フローのステップ4と「実験・検証の進め方」は「Codexによるレビューの実行方法」を名前で参照するのみで、旧方式の手順詳細を含まない（fmcs-utils も同構造のまま変更していない。節名は「…の実行方法（Herdr 対話方式）」となるが前方一致で参照は成立する）

### 本リポジトリ（DEV_TEMPLATE）自身への波及

ルート `CLAUDE.md` の「テンプレート改訂フロー」ステップ4は、`template/CLAUDE.md` のレビュー節を読み替えで適用する規定であり、旧方式の用語（バックグラウンド実行、`-o`（result.md）と full.log の分離、`resume` による逐次再レビュー）を名指ししている。テンプレート側の差し替えに伴い、この読み替え記述の更新が必要。

また Herdr 方式は定型指示をリポジトリ直下の `AGENTS.md` が供給する前提のため、本リポジトリ自身のレビュー運用にも `AGENTS.md` が必要になる。本案件のレビューを Herdr 方式で先行適用する（後述）にあたり AGENTS.md がレビュー開始前に存在しなければならないため、**ルート `AGENTS.md`（`template/AGENTS.md` と同一内容）は本案件の起票時（2026-08-27）に新設済み**（ユーザー指示）。反映作業・完了処理での作成は不要で、存在確認のみ行う。

## 反映対象の選別

| fmcs-utils 側の要素 | 取り込み | 理由 |
|---|---|---|
| レビュー節の全面差し替え（Herdr 対話方式・最終形） | ○（読み替え） | 方式自体は汎用。ストリーム命名のプロジェクト略称のみプレースホルダ化する |
| 「行き詰まり検出」節 | ○ | 完全にドメイン中立。fmcs-utils の文言そのまま |
| `docs/HERDR_SETUP.md` の配置 | ○ | プロジェクト固有記述なし。`template/docs/HERDR_SETUP.md` として同梱 |
| 周辺の整合修正4点（reviews/ 行・委任しない作業・update フロー ステップ4・Bash 例） | ○ | テンプレートにも同じ旧方式参照が存在する |
| fmcs-utils update-003 の2レーン方式 | × | 運用破綻が判明して廃止された方式。テンプレートは旧方式（exec+resume）から Herdr 方式へ直接移行し、2レーン方式は経由しない |
| fmcs-utils の実機検証記録・ドッグフーディング記録 | × | fmcs-utils 固有の検証履歴（本 README に要点のみ引用） |
| fmcs-utils の BACKLOG / CHANGELOG / 案件 README の完了処理 | × | fmcs-utils 固有の案件管理記録 |

## テンプレートへの適応（fmcs-utils との差分）

1. **ストリーム命名のプレースホルダ化**: fmcs-utils の `rev-fmcs-{案件ID}` は同一 PC 上の複数プロジェクト並存での名前衝突を防ぐためプロジェクト略称を含む。テンプレートでは `rev-{{プロジェクト略称}}-{案件ID}` とし、プロジェクト立ち上げ時に置き換える（ルート CLAUDE.md「テンプレートの使い方」手順2の既存規定でカバーされる）
2. **`template/.gitignore` の full.log 除外設定を削除**: Herdr 方式は full.log を生成しない。fmcs-utils は旧案件のファイルのために除外設定を残したが、テンプレートから立ち上げる新規プロジェクトに旧ファイルは存在しないため、行ごと削除する（本リポジトリのルート `.gitignore` は、既存案件のローカル full.log があるため fmcs-utils と同様に残す）
3. **本リポジトリの読み替え更新とルート AGENTS.md 新設**: 上記「本リポジトリ（DEV_TEMPLATE）自身への波及」のとおり

## 決定事項（fmcs-utils でのユーザー決定を踏襲）

以下は fmcs-utils update-004 でユーザーが決定済み（2026-08-27）の方針であり、テンプレートにもそのまま採用する:

1. **Herdr と codex を必須環境とする。フォールバックなし**（環境が満たされない場合はレビューを実施せず報告）
2. codex はレビューのたびに起動せず、生存確認のうえ再利用する
3. 排他の単位はストリーム（1ストリームに同時1依頼）。ストリーム間は並列可
4. 会話の区切りは `/new`（全文ゲートでの「新しい目」の確保）。同一会話の回数上限は設けない
5. サンドボックスは `--sandbox read-only --ask-for-approval never` を維持
6. 行き詰まり検出は閾値2回で全作業共通の必須ルール

## 本案件自身のレビュー方法

本案件のレビューは、**本案件で取り込む Herdr 対話方式を先行適用する**（fmcs-utils update-004 と同じドッグフーディング）。旧方式は fmcs-utils で運用破綻が判明しており、これを使う合理性がない。本セッションの Claude Code は Herdr 管理ペイン内で稼働しており（`HERDR_ENV=1`・ペイン `w2:p1` を確認済み）、実行可能である。

本リポジトリ向けの読み替え・注意:

- ストリーム名: `rev-devtmpl-update-004`（プロジェクト略称は `devtmpl` と読み替える）
- ルート `AGENTS.md` は起票時に新設済み（上記「本リポジトリ（DEV_TEMPLATE）自身への波及」参照）のため、レビュー依頼文は短縮形（定型指示なし）とし、回答冒頭の「[AGENTS.md適用]」マーカーを確認する（fmcs-utils の最終形と同じ運用）
- 結果は本案件 `reviews/codex-NN.result.md` に保存する（full.log は保存しない。過程は rollout jsonl と Herdr ペインに残る）。冒頭メタは新方式の形式（日付・対象・ストリーム名・フェーズ（反復/ゲート）・指摘数・トークン実測）
- レビュー対象は `README.md` と `design.md`、観点は本リポジトリ改訂フローの3点（矛盾・参照切れ / テンプレートとしての汎用性 / コピー直後にそのまま運用できるか）

### 適用結果（2026-08-27・収束済み）

4回（反復2回＋ゲート2回）で収束。全回で適用マーカー「[AGENTS.md適用]」を確認。手順どおりに運用できた。

| 回 | フェーズ | 結果 |
|---|---|---|
| codex-01 | 反復（初回） | 中1（検証 grep と差し替え文の full.log 矛盾） |
| codex-02 | 反復（解消確認） | 全解消・新規なし → /new |
| codex-03 | ゲート（/new 後） | 中1（`agent wait` のタイムアウト `{MS}` 未定義。反映元 fmcs-utils 原文由来の問題をゲートが検出） |
| codex-04 | ゲート（解消確認） | 全解消・新規なし → **収束** |

- 解消確認は diff 添付なしで成立した（codex が git diff・再読で自律的に照合）
- codex 側の一時的な容量エラー（Selected model is at capacity）が codex-03 の1回目の実行で発生。同一会話での再送で成功（会話・文脈の損失なし）
- トークン実測は各 `reviews/codex-NN.result.md` の冒頭メタに記録（反復会話 269,493 / ゲート会話 546,183。いずれも大半がキャッシュ済み入力）

## スコープ

### 変更対象

- `template/CLAUDE.md`: レビュー節の全面差し替え、「行き詰まり検出」節の新設、周辺の整合修正4点、ディレクトリ構成への `HERDR_SETUP.md` 追加
- `template/docs/HERDR_SETUP.md`: 新規（本案件フォルダの `HERDR_SETUP.md` をコピー）
- `template/.gitignore`: full.log 除外設定の削除
- ルート `AGENTS.md`: 新規（起票時に新設済み。`template/AGENTS.md` と同一内容）
- 完了処理: ルート `CLAUDE.md`（改訂フロー ステップ4の読み替え更新・ディレクトリ構成）、本案件 README

### スコープ外

- 機能追加・不具合修正フローのステップ4本文・「実験・検証の進め方」の変更（節名の前方一致参照で成立するため変更不要。fmcs-utils と同判断）
- `template/docs/codex-exec-ubuntu24-bwrap-fix.md` の削除（Herdr 方式でも codex のサンドボックスは bwrap を使うため、対策メモとして残す。fmcs-utils と同判断）
- 並列度の上限の規定（fmcs-utils 同様、運用実績を見て必要なら検討）
- 本リポジトリのルート `.gitignore` の変更（既存案件のローカル full.log のため除外設定を残す）
- fmcs-utils 側のファイルの変更（一切行わない）

## 関連ファイル

- 反映元: `/home/staff/sakagawa/git/fmcs-utils/CLAUDE.md`（最終形）、`/home/staff/sakagawa/git/fmcs-utils/docs/issues/update-004-herdr-review/`
- 反映先: `template/CLAUDE.md`、`template/docs/HERDR_SETUP.md`（新規）、`template/.gitignore`、ルート `CLAUDE.md`、ルート `AGENTS.md`（新規）
- セットアップ手順: 本案件フォルダの `HERDR_SETUP.md`
- 反映設計書: 本案件フォルダの `design.md`
