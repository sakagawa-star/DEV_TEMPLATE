# update-007 反映設計書: Herdr 方式移行後の本リポジトリ側の整合

## 1. 概要

ルート `CLAUDE.md` の「テンプレート改訂フロー」ステップ4にパスの読み替え2項目を追加し、ルート `.gitignore` に `.claude/handovers/` を追加する。テンプレート（`template/` 配下）は変更しない。

- **変更方式**: 既存箇条書きへの2項目追加＋1行追加（置換・削除なし。後処理なし）
- **実装者**: Claude Code 本体が直接編集する。理由: 変更対象がルートの `CLAUDE.md` と `.gitignore` のみであり、テンプレート改訂フローがサブエージェントに更新させる対象（`template/` 配下）を含まず、かつルート `CLAUDE.md` の編集はサブエージェントに禁じているため
- **テスト**: 不要（コード変更なし。自動・手動とも実施しない）

## 2. 変更対象ファイルと変更内容

### 2.1 ルート `CLAUDE.md` — ステップ4の読み替え箇条書きに2項目を追加

「テンプレート改訂フロー（update-XXX 案件）」ステップ4の箇条書きのうち、

```
   - ストリーム命名: `{{プロジェクト略称}}` は `devtmpl` と読み替える（例: `rev-devtmpl-update-004`）
```

の行の直後に、次の2行を挿入する:

```
   - パスの読み替え: 節内の `docs/HERDR_SETUP.md` は `template/docs/HERDR_SETUP.md`、`docs/codex-exec-ubuntu24-bwrap-fix.md` は `template/docs/codex-exec-ubuntu24-bwrap-fix.md` と読み替える
   - `docs/REVIEW_CRITERIA.md` の基準は本リポジトリでは適用せず、下記のレビュー観点3点の明示で代替する（従来どおりの運用）
```

他の箇条書き（保存先・レビュー対象・レビュー観点）は変更しない。

### 2.2 ルート `.gitignore` — `.claude/handovers/` の追加

`.claude/settings.local.json` の行の直後に次の1行を挿入する:

```
.claude/handovers/
```

変更後のファイル全文は次の3行となる:

```
.claude/settings.local.json
.claude/handovers/
docs/issues/*/reviews/*.full.log
```

`docs/issues/*/reviews/*.full.log` の行は変更しない（旧方式時代の案件のローカル full.log のために残置。update-004 で決定済み）。

## 3. 実施手順

1. 2.1 の2行を挿入する
2. 2.2 の1行を挿入する
3. 反映直後の検証を行う（4章）
4. 完了処理を行う（5章）

設計にない変更が必要になった場合は中断し、調査（テンプレート改訂フロー ステップ2）に戻る。

## 4. 反映直後の検証（完了処理の前）

1. `git diff CLAUDE.md` の差分が 2.1 の2行追加のみであること
2. `git diff .gitignore` の差分が 2.2 の1行追加のみであること（ファイル全文が 2.2 記載の3行と一致すること）
3. `git status --short` で、変更が `M CLAUDE.md`・`M .gitignore` と本案件フォルダ（未追跡）のみであること
4. ルート `CLAUDE.md` ステップ4の箇条書きが、ストリーム命名 → パスの読み替え → REVIEW_CRITERIA の代替 → 保存先 → レビュー対象 → レビュー観点、の6項目になっていること

## 5. 完了処理（4章の検証に合格し、人レビューを通過してから実施する）

Claude Code 本体が行う:

1. 本案件 `README.md` の `## ステータス` 節の本文を `Closed（2026-08-27 起票・完了）` に置き換える
2. ルート `CLAUDE.md` のディレクトリ構成: ファイルの追加はいずれも `docs/issues/` 配下のみのため更新不要（既存の記述形式では案件フォルダは例示1件のみ）
3. レビューストリーム `rev-devtmpl-update-007` に `/quit` を送って終了し、`herdr pane close {pane_id}` でペインを撤去する
4. git 操作（コミット）は本リポジトリ CLAUDE.md の運用ルールに従い Opus サブエージェントに委任する

## 6. テスト

コード変更がないため、テスト（自動・手動とも）は不要。
