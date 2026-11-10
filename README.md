# Independent Superpowers Model Routing

Superpowersの公式本体を無改変で利用し、親がサブエージェントの必要能力と契約に応じた予想総コストを比較するための独立したMarkdown群です。Copilot用資料に加え、Codex拡張向けの契約・指定経路・記録資料を追加しています。

**現在: 文書実装・架空データの算術確認・無改変本体との判断シミュレーションを確認済み。VS Code Local／CopilotとCopilot CLIの実機受入は未検証です。** 対応するアカウントのモデルID・容量・設定・実効結果を確認するまで、導入成功・節約額・全体移行完了を主張しません。

**Codex拡張: 入口・スキル読込と、2モデルの正式指定・runtime設定・fixture実行・子別トークン取得を確認済み。provider実効モデル・クレジット実費は未検証です。** [Codex検証記録](docs/superpowers/validation/codex-extension.md)と[試験開始手順](docs/model-routing-setup.md#8-codex拡張の隔離試験)を参照してください。CLIや別チャットの結果は拡張実機の証拠にしません。

## 導入

[導入・更新・解除手順](docs/model-routing-setup.md)に従って、利用先へ次を別々に配置します。

1. [公式Superpowers](https://github.com/obra/superpowers)を当該環境の公式導入経路で導入します。本体のスキルやキャッシュは編集しません。
2. [選択スキル](overlay/skills/model-selection/SKILL.md)を全参照資料込みで認識先へ配置します。Copilot個人配置は`~/.copilot/skills/model-selection/`、プロジェクト方式は対応する`.github/skills/model-selection/`。Codex隔離試験では`.agents/skills/model-selection/`です。
3. [共通入口](overlay/instructions/model-routing.md)の本文を、各環境が読む指示へ追加し、確認済みの配置先bindingを付けます。Codex隔離試験では`AGENTS.md`を使います。既存の指示は保持します。
4. 新しい会話で読込ソース・方針版・モデルの正式指定と実効値を確認します。同名の旧改変版や別のmodel-selectionコピーを混在させません。

`overlay/`は管理用原本であり、このリポジトリを開くだけでは全プロジェクトに適用されません。リンクやスキル一覧への表示だけでも取り込み・適用の証拠にはなりません。配置先と動作はharnessごとに確認します。

## 選択方針

- アカウント許可・必要能力・リスク・容量を先に満たし、入力・出力・キャッシュ・長文料金・修正／引き継ぎを含む予想総コストを比較します。
- 最終レビューにも最高能力モデルへの固定例外を作りません。本体の必須テスト・レビュー・独立性・修正上限は維持します。
- 正式な引数または確認済みのモデル固定workerで指定し、要求モデルと実効モデルを区別します。文章でモデル名を渡すだけでは設定になりません。
- 未観測値は`unknown`。指定経路なしと、指定可能だがメタデータ不明な場合を区別します。前者は当該派遣を保留します。
- 親が選び、料金表を子全員に配りません。スキル評価実験では指定モデルと設定をvariant間で固定します。

[料金カタログ](overlay/skills/model-selection/references/copilot-model-catalog.md)はCopilot AIクレジット契約用のスナップショットです。アカウント利用可否・VS Code ID・容量は未確認。別サービスの価格として使いません。候補更新は料金の公式根拠・確認日・版を伴い、モデル名から補完しません。鮮度目安30日、通常contextの余裕20%、数値予算は明示指定時のみです。

[Codexカタログ](overlay/skills/model-selection/references/codex-model-catalog.md)は別管理です。ChatGPT Plusの含有枠と追加クレジットを区別し、適用契約が未確認なら費用選択を保留します。明示的に承認された固定モデルの能力試験は費用選択と別記録にします。

## 対応と検証

| 対象 | 現在の証拠 |
| --- | --- |
| 公式原本 | v7.0.2 commit `22f0b463e7a5e579904dd6211a520321035526f7`。初期取込80ファイルと内容・mode完全一致 |
| 既存データ | カタログ／プロファイルのJSON等値とF01～F05の算術確認 |
| 判断 | 同じ無改変上流に入口を追加する対照・変更後各5sample。質的回帰確認 |
| 手動配置手順 | 一時ファイルで既存バイト保持・更新・ドリフト検出・解除を再現。実ユーザー設定は未変更 |
| VS Code Local／Copilot・CLI | 実機未検証。code/copilot実行ファイル・アカウント・実効情報なし |
| Codex拡張 | 入口／スキル読込（CX02）、2モデルの経路・runtime設定・fixture・子別トークン（CX03部分、CX07範囲限定）を確認。provider実効・費用・判断・解除・基準本体版互換性は未検証 |

[互換性記録](compatibility/superpowers.md)と[検証記録](docs/superpowers/validation/independent-model-routing.md)に条件と未確認項目を記載しています。文書・シミュレーション確認と実機受入を分け、本体／クライアントの版変更時に影響ケースを再確認します。常設パッチ・自動更新・独自ルーター・MCP・CLIラッパー・強制課金上限は追加していません。

## 旧方式からの移行

旧`.agents/skills/`の本体コピーはこの構成では廃止しました。旧実装は保存コミット`4792d5e`、移行前全体は`1a7d903`から取得できます。[移行台帳](docs/audits/2026-11-11-independent-model-routing-migration.md)に26変更・6追加・走査35項目・所有86ファイルの扱いを残しています。

利用先の切替は、検証した方式から行います。解除・復元は[手順書](docs/model-routing-setup.md)に従い、所有部分のハッシュを確認して独自追加部分だけを扱います。旧方式へ戻す場合も、公式版と旧改変版の同時読込を解消してから切り替えます。

[仕様書](docs/superpowers/specs/2026-11-11-independent-model-routing-design.md)・[実装計画](docs/superpowers/plans/2026-11-11-independent-model-routing.md)・[判断入力](tests/model-selection/independent-routing-scenarios.md)・[算術データ](tests/model-selection/scenarios.md)

旧仕様・計画・監査・検証文書は履歴です。旧配置へのリンクや行番号は調査時点を示し、現行の導入手順としては使いません。
