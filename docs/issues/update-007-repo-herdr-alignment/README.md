# update-007: Herdr 方式移行後の本リポジトリ側の整合（読み替え不足と .gitignore の補完）

## ステータス

Closed（2026-08-27 起票・完了）

## 目的・背景

update-004 で `template/CLAUDE.md` のレビュー節を Herdr 対話方式へ差し替え、本リポジトリ（DEV_TEMPLATE）自身もルート `CLAUDE.md` の「テンプレート改訂フロー」ステップ4の読み替えで同節を適用する運用に移行した。移行後にテンプレートと本リポジトリの運用ドキュメントを突き合わせた結果、本リポジトリ側に未反映の不整合が2点見つかった。本案件で解消する。

本案件はテンプレート（`template/` 配下）を変更しない。変更対象はルートの `CLAUDE.md` と `.gitignore` のみ（本リポジトリ自身の運用ドキュメントの改訂）。

## 調査結果

### 不整合1: レビュー節が参照するパスの読み替え不足（ルート `CLAUDE.md` ステップ4）

本リポジトリの `docs/` 直下には `issues/` しか存在しないため、`template/CLAUDE.md` の「Codexによるレビューの実行方法（Herdr 対話方式）」節を読み替えで適用した際、節内の次の参照が本リポジトリでは解決しない:

| 節内の参照 | 参照箇所 | 本リポジトリでの実体 |
|---|---|---|
| `docs/HERDR_SETUP.md` | 前提環境（セットアップ手順） | `template/docs/HERDR_SETUP.md` |
| `docs/codex-exec-ubuntu24-bwrap-fix.md` | bwrap 注記 | `template/docs/codex-exec-ubuntu24-bwrap-fix.md` |
| `docs/REVIEW_CRITERIA.md` | レビューの進め方（「基準に従うこと」） | `template/docs/REVIEW_CRITERIA.md`。ただし本リポジトリの update 案件レビューは従来からレビュー観点3点の明示で代替しており（update-004 のドッグフーディングも同様）、REVIEW_CRITERIA は適用していない。この代替関係が明文化されていない |

ルート `CLAUDE.md` ステップ4の読み替え箇条書きには現在、ストリーム命名・保存先・レビュー対象・レビュー観点の4項目しかなく、パスの読み替えがない。

### 不整合2: ルート `.gitignore` に `.claude/handovers/` がない

- `template/.gitignore` は `.claude/settings.local.json` と `.claude/handovers/` を除外する
- 本リポジトリの `.gitignore` は `.claude/settings.local.json` と `docs/issues/*/reviews/*.full.log` の2行のみで、`.claude/handovers/` がない
- 一方、本リポジトリの git 運用（`template/CLAUDE.md` の「git 操作の実行方法」をそのまま適用）は「`.claude/handovers/` 配下はコミットに含めない」を前提とする。`/handover` を本リポジトリで使うと未追跡ファイルとして現れ、誤ステージのリスクがある
- なお `docs/issues/*/reviews/*.full.log` の行は、旧方式時代の案件（update-001〜003、update-005/006）のローカル full.log のために意図的に残す（update-004 README「テンプレートへの適応」で決定済み）

## 反映対象の選別

| 項目 | 対応 | 理由 |
|---|---|---|
| ステップ4へのパス読み替えの追加（HERDR_SETUP / bwrap メモ） | ○ | 参照が解決しない状態の解消。実体パスへの読み替えを明文化する |
| `docs/REVIEW_CRITERIA.md` の扱いの明文化 | ○ | 従来運用（観点3点で代替）を明文化し、節の記述との矛盾を解消する |
| `.gitignore` への `.claude/handovers/` 追加 | ○ | git 運用ルールの前提と `.gitignore` の実態を一致させる |

### 検討したが本案件では見送った事項（2026-08-27・ユーザーへ提示済み）

1. **行き詰まり検出のルート運用ルールへの明示参照**: 現状はレビュー節経由で適用される。明示は将来の改訂候補
2. **セッション引き継ぎ節（handover 運用）のルートへの追加**: テンプレート CLAUDE.md 冒頭にはあるがルートにはない。将来の改訂候補
3. **本リポジトリ自身の BACKLOG.md の導入**: 案件ステータスは各案件 README で管理しており、現状不足はない。将来の改訂候補

## スコープ

### 変更対象

- ルート `CLAUDE.md`: 「テンプレート改訂フロー」ステップ4の読み替え箇条書きに2項目を追加
- ルート `.gitignore`: `.claude/handovers/` の1行を追加

### スコープ外

- `template/` 配下の変更（一切行わない）
- 上記「検討したが本案件では見送った事項」の3点

## 本案件自身のレビュー方法

「Codexによるレビューの実行方法（Herdr 対話方式）」をルート `CLAUDE.md` ステップ4の読み替えで適用する（update-004 で確立した運用）。ストリーム名は `rev-devtmpl-update-007`。依頼文は短縮形（定型指示はルート `AGENTS.md` が供給）とし、回答冒頭の「[AGENTS.md適用]」マーカーを確認する。結果は本案件 `reviews/codex-NN.result.md` に冒頭メタ付きで保存する。

## 関連ファイル

- 変更対象: ルート `CLAUDE.md`、ルート `.gitignore`
- 参照: `template/CLAUDE.md`（レビュー節）、`template/.gitignore`、`docs/issues/update-004-herdr-review/README.md`（Herdr 方式移行の経緯）
- 反映設計書: 本案件フォルダの `design.md`
