# update-005: fmcs-utils のステータス凡例（On Hold 含む）をテンプレートへ取り込み

## ステータス

Closed（2026-08-27 起票・完了）

## 目的・背景

fmcs-utils の update-002（ステータス凡例に On Hold を追加）で確立した BACKLOG のステータス凡例運用を、テンプレートへ取り込む。

fmcs-utils では、案件を一時中止する状態を表すステータスが凡例に存在しなかったため、凡例未定義の「凍結」表記が先行使用される不整合が発生した（fmcs-utils feat-007）。この経験から、**On Hold**（一時中止。備考に日付・理由・再開点を記録するルール付き）が凡例に正式追加された。

現行テンプレートの `template/docs/BACKLOG.md` には**ステータス凡例の節自体が存在せず**、記入例の HTML コメント内に「ステータス: Open / In Progress / Closed」と記載があるのみである。このままでは、テンプレートから立ち上げた新規プロジェクトが fmcs-utils と同じ「凡例未定義のまま先行使用」問題を踏む。

## 反映元

- リポジトリ: `/home/staff/sakagawa/git/fmcs-utils`（コミット `8868925ee3ffd351d6456ebf83871401e0712740`、2026-08-27 時点）
- 案件: `docs/issues/update-002-add-onhold-status/`（README.md / design.md）
- 反映後の実ファイル: `docs/BACKLOG.md` の `## ステータス凡例` 節

## 調査結果

### fmcs-utils 側の凡例（反映後の全文）

`docs/BACKLOG.md` 末尾の `## ステータス凡例` 節に以下の6ステータスが定義されている:

- **Open**: 起票済み・未着手
- **In Progress**: 調査・実装中
- **Review**: レビュー中
- **On Hold**: 一時中止（凍結）。再開する場合も、そのまま中止する場合もある。On Hold にする際は、本表の備考に日付・理由・再開点を記録し、案件フォルダの README.md のステータスも On Hold に更新する
- **Closed**: 完了
- **Cancelled**: 取りやめ・破棄（ドキュメントは履歴として残す）

ステータス名を「On Hold」とした選定理由（Suspended / Frozen / Deferred との比較）は fmcs-utils update-002 README.md に記録済み。

### テンプレート側の現状

- `template/docs/BACKLOG.md`: 凡例節なし。記入例コメント（L12〜L19）内に「ステータス: Open / In Progress / Closed」の1行のみ。Review / On Hold / Cancelled は未定義
- `template/CLAUDE.md`: ステータス名への言及は各フローの完了ステップの「ステータスを Closed に更新する」のみ。凡例追加による変更は不要（fmcs-utils update-002 の調査結果と同一構造）
- `template/docs/CHANGELOG.md`: ステータスへの言及なし

## 反映対象の選別

| fmcs-utils 側の変更 | 取り込み | 理由 |
|---|---|---|
| `docs/BACKLOG.md` の `## ステータス凡例` 節（6ステータス） | ○ | 完全にドメイン中立。On Hold 1行だけでなく凡例節ごと導入することで、新規プロジェクトが最初から全ステータスを定義済みの状態で運用を始められる |
| feat-007 の「凍結」→「On Hold」表記置換 | × | fmcs-utils 固有の過去案件の表記修正 |
| fmcs-utils の BACKLOG / CHANGELOG / 案件 README の完了処理 | × | fmcs-utils 固有の案件管理記録 |

### テンプレートへの適応（fmcs-utils との差分）

fmcs-utils の凡例は On Hold 時に「案件フォルダの README.md のステータスも On Hold に更新する」と定めるが、feat フロー（テンプレート・fmcs-utils とも同一構造）は案件作成時に `requirements.md` / `design.md` の作成のみを要求し、案件 README.md の作成を必須としていない（bug / update フローは要求する）。このため凡例をそのまま転用すると、feat 案件では更新対象の README.md が存在しない場合がある。実際 fmcs-utils の feat-007 では、一時中止の時点で README.md を作成してステータスを記録した。

この実績に合わせ、テンプレート版の凡例では On Hold の記録ルールに「README.md が未作成の案件では、概要と現在のステータスを記した README.md を作成して記録する」を追記して整合させる。フロー側（feat の案件作成ステップ）で README.md 作成を必須化する案は、反映元案件のスコープを超えるためスコープ外とする（別案件候補）。

## スコープ

### 変更対象

- `template/docs/BACKLOG.md`: `## ステータス凡例` 節の新設と、記入例コメント内のステータス行の凡例参照への変更

### スコープ外

- `template/CLAUDE.md` の変更（ステータス名への言及は完了ステップの「Closed に更新」のみで、凡例追加による変更は不要）
- 各フロー（feat）の案件作成ステップでの README.md 作成の必須化（反映元案件のスコープ外。別案件候補）
- fmcs-utils 側のファイルの変更（一切行わない）

## 関連ファイル

- 反映元: `/home/staff/sakagawa/git/fmcs-utils/docs/issues/update-002-add-onhold-status/`
- 反映先: `template/docs/BACKLOG.md`
- 反映設計書: 本案件フォルダの `design.md`
