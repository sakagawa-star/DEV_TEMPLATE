# codex-01: update-012 初回レビュー（A）

- 日付: 2026-09-06
- 対象ファイル: `docs/issues/update-012-import-provenance-rule/README.md`、`docs/issues/update-012-import-provenance-rule/design.md`
- ストリーム: `rev-devtmpl-update-012`（ペイン w4:pC。新規起動。update-011 で改訂したルート `AGENTS.md`〔規定適合確認あり〕を読んだ最初のストリーム）
- 依頼種別: A（初回レビュー）
- 直前に `/new` を送ったか: No（新規起動）
- ゲート状態: 未実施
- 指摘数: 高 1 / 中 0 / 低 0
- 収束判定: 未収束（次: 全件反映 → B）
- トークン実測（rollout jsonl の `total_token_usage` 累積）: input 258,553（うち cached 204,032）/ output 3,287（reasoning 1,706）/ total 261,840
- rollout jsonl: `~/.codex/sessions/2026/09/06/rollout-2026-09-06T21-43-43-01a076be-fe05-74a1-b854-98f577e70856.jsonl`

## 依頼文

> 以下のドキュメントをレビューせよ: docs/issues/update-012-import-provenance-rule/README.md と docs/issues/update-012-import-provenance-rule/design.md 。本案件は本リポジトリ発の改訂（反映元リポジトリなし）で、ルートの CLAUDE.md「テンプレート改訂フロー」ステップ2・3、ルートの AGENTS.md、template/CLAUDE.md「ドキュメント更新フロー」を変更する反映設計である。design.md の変更前テキストは現行の各ファイルに一致している必要がある。レビュー観点は次の3点: (1) ドキュメント間の矛盾・参照切れ（存在しないファイル・セクションへの参照、フロー番号のずれ。変更後テキストを適用した後のルート CLAUDE.md・ルート AGENTS.md・template/CLAUDE.md の内部整合を含む）(2) テンプレートとしての汎用性（プロジェクト固有記述の混入、プレースホルダの不足・不整合）(3) 新規プロジェクトへコピーした直後にそのまま運用できるか。

## レビュー結果

[AGENTS.md適用]

高: 本リポジトリ発の改訂を許容する D1 と、案件名の規定が矛盾しています。D1 は「反映元リポジトリがない案件」を正式に扱いますが、変更後もステップ1は slug に反映元が分かる名前を必須とします。そのため本案件の update-012-import-provenance-rule 自体が規定に適合せず、以後の本リポジトリ発案件にも命名不能な状態が残ります。

修正案: D1 にステップ1も含め、「反映元がある場合は反映元が分かる slug、ない場合は変更目的が分かる slug」と明記してください。設計箇所 (docs/issues/update-012-import-provenance-rule/design.md:26)／現行規定 (CLAUDE.md:59)

上記以外は問題ありません。案件フォルダには必須の README.md と design.md が揃っており、変更前テキスト4箇所は現行ファイルと一致しています。変更後のテンプレート文面にも、プロジェクト固有の記述や参照切れは見当たりません。

## 動作確認（update-011 design.md「6. 動作確認」）

- 「[AGENTS.md適用]」マーカーあり。回答に案件フォルダ単位の適合判定（必須の README.md と design.md の存在）が含まれており、update-011 で追加したルート `AGENTS.md` の規定適合確認が有効になっていることを確認した

## 対応

- 高: D1 の範囲をステップ1〜3 に広げ、ステップ1 の slug 規定を「反映元がある場合は反映元がわかる名前、ない場合（本リポジトリ発の改訂）は変更の目的がわかる名前」に改めた（既存の update-004 / 007 / 012 が後者の前例）。方針・検証2・改訂履歴、README の調査結果を同期
- 同じ会話で解消確認（codex-02）を依頼
