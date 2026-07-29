# update-002-from-synchrocap

## 概要

SynchroCap で運用実績のできた「ドキュメント更新フロー（update-XXX 案件）」を汎用化し、`template/CLAUDE.md` に取り込む案件。

- **反映先**: `template/CLAUDE.md`（および軽微な追随として `template/docs/BACKLOG.md` の記入例）
- **ステータス**: Closed（2026-07-30）
- **種別**: Update（テンプレート改訂）

## 反映元

- **リポジトリ**: `/home/sakagawa/git/SynchroCap`
- **コミット**: `dd8fb8a879c1d8affce27c4d5f086292cf6d82b2`（2026-07-28、update-003: ドキュメント更新フロー（update-XXX 案件）の明文化）
- **参照ドキュメント**:
  - `CLAUDE.md` の「### ドキュメント更新フロー（update-XXX 案件）」セクションと、update-003 で行われた整合修正箇所
  - `docs/issues/update-003-doc-update-flow/README.md` / `design.md`（フロー明文化の経緯と設計判断）

## 調査結果（差分の全量）

SynchroCap 側で update-003 により追加・修正された内容は以下のとおり。テンプレート（`template/CLAUDE.md`）にはいずれも存在しない。

1. **「### ドキュメント更新フロー（update-XXX 案件）」セクションの新設**（不具合修正フローの直後）
   - 適用範囲: 運用ドキュメント（CLAUDE.md、docs/ 直下の基準書・BACKLOG・CHANGELOG、.gitignore 等）の改訂。コード変更は含まず、必要と判明したら中断して feat/bug へ起票し直す。個別機能ドキュメントの修正は元案件側で扱う
   - 要求仕様書・機能設計書は作らず、README.md（調査）＋ design.md（反映設計）の2点で代替
   - フロー7ステップ: 案件作成 → 調査（README.md、反映先は編集しない）→ 設計・保存（design.md、自己完結・完了処理込み）→ レビュー（Codex→人、観点3点明示: 自己完結性・情報の喪失・ドキュメント間整合性）→ 反映（design.md 厳密準拠、本体実施が原則、`git diff` 検証）→ 完了（BACKLOG・CHANGELOG・案件 README 更新）→ テスト不要の明記
   - 運用メモ: 汎用改善の DEV_TEMPLATE への還元検討、グローバル gitignore による `.gitignore` の `git add -f` 注意
2. **既存箇所の整合修正**（update-003 design.md §2.3）
   - (1) 「ドキュメント作成ルール」冒頭: 必須ドキュメントを案件種別（feat / bug / update）ごとに定義する形へ変更
   - (1b) 同セクションの配置ルール行: ファイル名の列挙をやめ「案件種別ごとの必須ドキュメント定義に従う」へ変更
   - (2) 「案件管理」の BACKLOG.md 説明行: 案件種別の列挙を feat / bug / update / inv に拡張
   - (3) 「案件ディレクトリ構成」の構成図: 例に `update-001-zzz` を追加し、構成図直後に「update 案件は requirements.md / investigation.md を持たず、README.md・design.md・reviews/ で構成される」の注記を追加

## 取り込む / 取り込まない の選別

### 取り込む（汎用化して反映）

| 項目 | 汎用化の内容 |
|---|---|
| ドキュメント更新フローのセクション全体 | 下記の読み替えを除きそのまま取り込む |
| ステップ1の BACKLOG 追加 | SynchroCap の「Open 表に追加（Type: Update）」は Open/Closed 2表構成（SynchroCap update-002 で確立）が前提。テンプレートの `docs/BACKLOG.md` は単一表＋ステータス列のため「`docs/BACKLOG.md` に追加する」とする（feat/bug フローのステップ1と同一表現） |
| ステップ6の完了処理 | 同上の理由で「Open 表から削除し Closed 表へ移動」は取り込まず、「`docs/BACKLOG.md` のステータスを Closed に更新する」とする（feat/bug フローのステップ8と同一表現）。「ファイルの追加・削除があった場合は CLAUDE.md のディレクトリ構成を更新」は取り込む |
| 適用範囲のコードディレクトリ列挙 | 「`src/`, `tools/`, `tests/`」は SynchroCap 固有のため「ソースコード・テストコード」という汎用表現にする |
| ステップ7のテスト不要 | 「pytest・実機テスト」は「テスト（自動・手動とも）」という汎用表現にする |
| 典型例の DEV_TEMPLATE 取り込み | 趣旨は保持するが、`~/git/DEV_TEMPLATE/template` という固定パスは利用者環境に存在する保証がないため「本プロジェクトのコピー元テンプレートリポジトリ」という汎用表現に置換する（Codex レビュー指摘対応） |
| 運用メモの DEV_TEMPLATE 還元 | 趣旨は保持し、固定パスを使わず「コピー元の開発ドキュメントテンプレートリポジトリ」という汎用表現にする（同上） |
| 運用メモの `git add -f` 注意 | 「update-001 で追跡開始済み」という SynchroCap 固有の実績言及を削除して汎用化する |
| 整合修正 (1)(1b)(3) | テンプレートの対応箇所（「ドキュメント作成ルール」「案件ディレクトリ構成」）にそのまま適用する |
| （追加）「Codexによるレビューの実行方法」の整合修正 | 反映元 SynchroCap には存在しない修正だが、同セクションが feat/bug 専用の説明のままだと update 案件のレビュー手順が欠けるため、冒頭文の拡張と「初回レビュー（ドキュメント更新の場合）」のコマンド例を追加する（Codex レビュー指摘対応） |
| BACKLOG 雛形の記入例 | `template/docs/BACKLOG.md` の記入例コメントに update 案件の行例を1行追加する（update 案件の存在を雛形からも発見可能にするため） |

### 取り込まない

| 項目 | 理由 |
|---|---|
| 整合修正 (2)（案件管理セクションの BACKLOG 説明行） | テンプレートの CLAUDE.md には「案件管理」セクション自体が存在しない（案件一覧は「完了済み案件」セクションで BACKLOG.md を参照する構成）。対応箇所がないため対象外 |
| BACKLOG の Open/Closed 2表構成（SynchroCap update-002 相当） | 本案件のスコープ外。テンプレートの BACKLOG は単一表＋ステータス列で成立しており、2表構成の導入は別の改善提案として扱うべき |
| inv（調査）案件種別 | SynchroCap 固有の運用でテンプレートには未導入。本案件のスコープ外 |

## 本リポジトリの CLAUDE.md への影響

DEV_TEMPLATE ルートの `CLAUDE.md` にある「テンプレート改訂フロー（update-XXX 案件）」は本案件の反映対象ではない（すでに同型のフローを持ち、`template/CLAUDE.md` を参照する読み替え方式のため変更不要）。

## Codex レビュー

`reviews/` に結果を保存する（レビュー対象: 本 README.md と design.md）。
