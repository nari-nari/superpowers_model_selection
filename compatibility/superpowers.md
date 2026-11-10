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
| Codex VS Code拡張 | 26.51021.22027、engine 0.164.0-alpha.11.1 | 7.0.1 manifest宣言 | Sol／LunaのUI選択可を確認。子の実効ID未取得 | unavailable | 準備・入口／スキル読込確認済み。モデル適用未検証 |

文書・シミュレーション確認と実機確認は別。従来のLocal／Copilot／CLIの3方式の必須受入が揃うまで、その移行を完了としない。Codexの確認は代替証拠にしない。未知の本体／クライアント版は未検証に戻し、派遣・モデル指示・レビュー・修正上限・ヘルパーの差分を読む。更新の自動適用・自動検知は提供しない。

実機の2候補、読み込み、Auto／拒否／上書き、context／effort、解除は[新検証記録](../docs/superpowers/validation/independent-model-routing.md)へ証拠を残す。観測不能な項目はunknown、再現不能な受入はunverified。現行環境にcopilot/codeの実行ファイルがなく、ネイティブ確認は未実施。

## 管理リポジトリの状態

旧.agents/skillsの所有86ファイルを廃止。本体コピーなし。現在overlayは入口1資料とスキル8資料の計9資料で、上流原本へのパッチなし。Copilotの3環境は未展開・未検証のまま。

## Codex拡張の確認範囲

| 範囲 | 状態 | 根拠 |
| --- | --- | --- |
| 準備 | 準備・読込確認済み | 契約別参照、8ファイル配布、所有ハッシュ、リンク、解除の一時再現。CX02は拡張session metadata・指示本文・読込call/outputで確認 |
| 判断 | 未検証 | 参照取得sampleは判断全体や本体ワークフローの実機確認ではない |
| モデル適用 | 未検証 | 正式スキーマと候補2件のUI選択可は確認。子の呼出し・実効IDは未取得 |
| 費用比較 | 未検証 | ChatGPT Plus確認のみ。使用枠・speed・呼出し帰属情報が未確認 |

稼働拡張版はユーザーが同じwindowのRunning Extensionsで確認、エンジン版・入口取込は対象拡張会話のネイティブログで照合した。拡張本体の有効ソースはhost-skills対応で確認したv7.0.1で、上流commit・原本一致はunknown。基準v7.0.2との差とfull-history overrideの文書矛盾を保持し、本体を自動更新・編集していない。[Codex検証記録](../docs/superpowers/validation/codex-extension.md)のCX01～CX10で個別に判定する。
