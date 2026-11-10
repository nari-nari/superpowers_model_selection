# Superpowers Custom

GitHub Copilot のモデル料金を考慮する Superpowers カスタムスキル群です。親エージェントが、必要な能力を満たす候補から予想総コストで選びます。入力・出力・キャッシュ・長文料金・再試行と引き継ぎを比較し、選択理由と要求／実効モデルを区別します。

**現在の状態: 文書実装と読み取り専用の選択シミュレーションを確認済み。VS Code Local／Copilot と Copilot CLI の実機受入は未検証です。** 対象環境の `copilot`／`code` コマンドをこの作業環境では利用できません。料金表の掲載は、契約での利用可否やネイティブ派遣の成功を意味しません。

## 導入

1. このリポジトリの `.agents/skills/` をカスタム版の原本として使います。VS Code では選択中の harness の Agent Customizations／Skills で、このコピーの `using-superpowers` と `model-selection` を認識していることを確認します。同名のプラグイン版と混在する場合は、実際に読み込むソースを確認し、変更したコピーを使うよう指定してください。
2. VS Code Local と CLI の対応するスキル配置は環境の公式仕様で確認してください。Copilot の標準配置 `.github/skills/` が必要な場合は、利用先リポジトリのその配置へカスタムスキル一式をコピーします。`SKILL.md` だけでなく参照資料・テンプレート・既存スクリプトも含め、相対関係を維持します。既存の同名コピーを自動上書きするインストーラーはありません。
3. [VS Code ガイド](.agents/skills/using-superpowers/references/vscode-tools.md) または [CLI ガイド](.agents/skills/using-superpowers/references/copilot-cli-tools.md) に従い、利用可能な正確なモデルID、ウィンドウ／出力上限、推論設定、モデル指定の適用方法を確認します。モデルIDと `availability` 等を、根拠・確認日付きでカタログまたはセッションの上書き記録に補います。未知の値を有効と仮定しません。
4. 呼び出し単位のモデル指定がない環境に限り、ガイドの雛形からモデル指定済み custom worker を作り、認識と適用を確認します。雛形は未登録で、プレースホルダーを含むまま実行する設定ではありません。
5. 新しい会話で2候補を使った短い読み取り依頼を行い、実際の要求／実効モデルを比較します。自動選択やユーザー設定による上書き、拒否、メタデータ不明も記録します。この確認を終えていない方式は「未検証」のままです。

MCP サーバー・ユーザー共通設定・親モデルの自動切り替え・CLI 子セッションのラッパーは追加していません。必要な読み取り／派遣ツールは、利用中の環境で使えるネイティブ機能を使用します。

[VS Code 公式のスキル配置・認識確認](https://code.visualstudio.com/docs/agent-customization/agent-skills) · [CLI 公式リファレンス](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

## 使い方

カスタム版を読み込んだ親エージェントへ、例えば次のように依頼します。

> このリポジトリのカスタム Superpowers を使って作業してください。子のモデルは model-selection に従い、必要な能力と予想総コストで選択し、要求したモデルと実際の適用結果を記録してください。

[共通ルール](.agents/skills/model-selection/SKILL.md) は、実装・最終レビュー・再レビュー・設計確認・調査・診断に接続しています。スキルの評価テストは、安さより実験で指定したモデルと設定の固定を優先します。子へ料金表を丸ごと配布したり、見積もりのために別の子を起動したりはしません。

記録は既存 ledger／作業ログへ短く追記します。

```yaml
task: task-1 / review
runtime: copilot-cli
catalog_revision: 2026-11-10.1
profile: task-review
model_requested: '<この環境で確認した正確なID>'
model_effective: unknown
effort_requested: default
context_requested: default
context_effective: unknown
estimate_credits: unknown
reason: 適性と利用可否を確認した候補から、必要な入出力と課金階層で比較する
actual_cost: unknown
evidence: unavailable
```

これはメタデータ未取得時の例です。`unknown` を選択適用済み・実費取得済みと読み替えません。正式なモデル指定ができなければ、影響する派遣だけを保留して設定方法を確認します。

## 料金・候補を更新する

[カタログ](.agents/skills/model-selection/references/copilot-model-catalog.md) が料金と識別情報の原本、[作業プロファイル](.agents/skills/model-selection/references/task-profiles.md) が能力要件との対応です。初期候補は3モデルで、公式料金とCLI IDは2026-11-10時点。ユーザー環境の利用可否・VS Code ID等は未確認です。高リスク作業の候補は、その領域の適性根拠を確認してから登録します。

- [Copilot公式料金表](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing) でモデル別の単価・閾値・適用範囲を確認し、確認日と `catalog_revision` を更新します。別サービスのAPI料金で補完しません。
- 鮮度の目安は30日。期限超過は一度更新を試み、取得不能なら古い見積もりであることを一度通知します。未知の単価は無料ではありません。
- 通常コンテキストを標準とし、予測ピークに対して20%の余裕を見ます。大きい設定の必要性と適用可否を分けて判断します。
- 数値予算は任意。指定された予算を見積もりが超える場合に当該派遣を確認します。請求額の強制上限や節約率の保証は提供しません。

旧プレミアムリクエスト課金、他の契約の料金、未確認の割引はこの料金表の対象外です。別環境ではその契約に合う根拠を使用し、価格比較ができない場合はその制限を明示します。

## 検証資料

- [検証結果と未検証項目](docs/superpowers/validation/cost-aware-model-selection.md): 35項目の移行先、20受入条件、実機確認手順。
- [再現用の架空データとケース](tests/model-selection/scenarios.md): 実モデルに課金せず算術・境界値を確認する入力。算術の実行コマンドは[実装計画 Task 1](docs/superpowers/plans/2026-11-10-cost-aware-model-selection.md)にあります。
- [仕様](docs/superpowers/specs/2026-11-10-cost-aware-model-selection-design.md)と[当初の走査](docs/audits/2026-11-05-model-selection-audit.md)。走査の行番号は変更前の記録です。

このリポジトリは Markdown を中心とした指示の変更です。独自の推論ルーター、定期料金取得、請求APIクライアント、実行時の依存パッケージは含みません。
