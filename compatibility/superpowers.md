# Superpowers互換性記録

確認日: 2026-11-11。方針版: `2026-11-11.2`。料金表版: Copilot `2026-11-10.1`、Codex `2026-11-11.codex.1`。

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
| Codex VS Code拡張 | 稼働版unknown | unknown | unknown | unavailable | 準備済み・実機未検証 |

文書・シミュレーション確認と実機確認は別。従来のLocal／Copilot／CLIの3方式の必須受入が揃うまで、その移行を完了としない。Codexの確認は代替証拠にしない。未知の本体／クライアント版は未検証に戻し、派遣・モデル指示・レビュー・修正上限・ヘルパーの差分を読む。更新の自動適用・自動検知は提供しない。

実機の2候補、読み込み、Auto／拒否／上書き、context／effort、解除は[新検証記録](../docs/superpowers/validation/independent-model-routing.md)へ証拠を残す。観測不能な項目はunknown、再現不能な受入はunverified。現行環境にcopilot/codeの実行ファイルがなく、ネイティブ確認は未実施。

## 管理リポジトリの状態

旧.agents/skillsの所有86ファイルを廃止。本体コピーなし。現在overlayは入口1資料とスキル8資料の計9資料で、上流原本へのパッチなし。Copilotの3環境は未展開・未検証のまま。

## Codex拡張の確認範囲

| 範囲 | 状態 | 根拠 |
| --- | --- | --- |
| 準備 | 準備済み | 契約別参照、8ファイル配布、所有ハッシュ、リンク、解除の一時再現 |
| 判断 | 未検証 | 参照取得sampleは判断全体や本体ワークフローの実機確認ではない |
| モデル適用 | 未検証 | 拡張の読込・正式指定と実効IDが未取得 |
| 費用比較 | 未検証 | ChatGPT Plus確認のみ。使用枠・speed・呼出し帰属情報が未確認 |

インストールファイルの2版は稼働版の証拠にしない。この会話に見える本体v7.0.1のパスも拡張本体の有効版を証明しない。基準v7.0.2へ自動更新していない。[Codex検証記録](../docs/superpowers/validation/codex-extension.md)のCX01～CX10で個別に判定する。
