# Superpowers互換性記録

確認日: 2026-11-11。方針版: `2026-11-11.1`。料金表版: `2026-11-10.1`。

## 原本

- 取得元: [obra/superpowers](https://github.com/obra/superpowers)
- 検証基準: `v7.0.2`、コミット`22f0b463e7a5e579904dd6211a520321035526f7`（注釈タグをpeelしたcommit ID）。
- skills tree: `fcfa2f8a624b4cd711cbade5bd3d89d43a52cfe4`。初期取込の全80ファイルと内容・mode一致。本体は別途導入し、このリポジトリには同梱しない。
- [出自・移行台帳](../docs/audits/2026-11-11-independent-model-routing-migration.md)

## 実行環境別

| 環境 | クライアント版 | 導入本体版 | 確認済みID | 実効ID証拠 | 状態 |
| --- | --- | --- | --- | --- | --- |
| VS Code Local | unknown | unknown | unknown | unavailable | 未検証 |
| VS Code Copilot | unknown | unknown | unknown | unavailable | 未検証 |
| Copilot CLI | unknown | unknown | unknown | unavailable | 未検証 |

文書・シミュレーション確認と実機確認は別。上記3方式の必須受入が揃うまで全体移行完了としない。未知の本体／クライアント版は未検証に戻し、派遣・モデル指示・レビュー・修正上限・ヘルパーの差分を読む。更新の自動適用・自動検知は提供しない。

実機の2候補、読み込み、Auto／拒否／上書き、context／effort、解除は[新検証記録](../docs/superpowers/validation/independent-model-routing.md)へ証拠を残す。観測不能な項目はunknown、再現不能な受入はunverified。現行環境にcopilot/codeの実行ファイルがなく、ネイティブ確認は未実施。
