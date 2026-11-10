# Codex拡張の検証記録

日付: 2026-11-11（Asia/Tokyo）。[仕様](../specs/2026-11-11-codex-extension-validation-design.md)・[計画](../plans/2026-11-11-codex-extension-validation.md)はユーザー承認済み。開始コミット `e29b793bd417b7413f01a5171d4e212d90d03279`。

## 実行環境と範囲

管理変更は隔離branch `feat/codex-extension-validation`。元mainと元の未追跡仕様・計画を保持。ChatGPTアカウントによるPlusログインはユーザー回答で確認。現在利用中の枠が含有枠か追加クレジットか、speedと残量、対象アカウントの実モデルIDは未確認。

管理側のこの会話は拡張由来と確認できていない。GUI操作ツールなし。CLIで推論を実行して代理試験にはしない。別のCodex拡張会話から事前確認結果を受領し、そのsession metadataと指示本文を独立照合した。稼働拡張はユーザーのRunning Extensions確認により `26.51021.22027`、当該拡張会話のエンジンは `0.164.0-alpha.11.1`。拡張側の有効Superpowersはmanifest宣言v7.0.1で、上流commit・原本一致・基準v7.0.2との互換性は未検証。

## 受入台帳

| ID | 文書・ローカル確認 | 拡張実機 | 理由／証拠 |
| --- | --- | --- | --- |
| CX01 | 拡張会話の出所と稼働版・エンジン版・本体の有効ソースを確認 | unverified | VS Code稼働版・上流commit・基準版互換性は未取得。後述の部分証拠を保持 |
| CX02 | 所有ハッシュと原本を照合 | pass | native session metadata・world_state・読込call/outputを独立確認。確認した会話と列挙ソースの範囲に限定 |
| CX03 | 正式スキーマ、2候補と同一windowのUI利用可否を確認 | unverified | 2子の呼出し・独立した実効IDはまだ未取得 |
| CX04 | 拒否・fallback・未知設定の期待動作を定義 | unverified | 拡張で未再現 |
| CX05 | 既存S01・S02・S05・S08を再利用 | unverified | 実機の判断試験未実施 |
| CX06 | Plusをruntimeでも確認。追加クレジットなしの観測あり | unverified | 含有枠を支持するが応答単位bucket・換算根拠は未取得 |
| CX07 | 呼出し前後の採取手順を準備 | unverified | 呼出しなし。usage／actual_costはunknown |
| CX08 | S06との比較手順を準備 | unverified | 拡張側本体ワークフロー未確認 |
| CX09 | 所有物の配置・解除確認を準備 | unverified | 実機解除後の新会話未確認 |
| CX10 | S07と復元手順を準備 | unverified | 拡張で再開・圧縮未再現 |

実機passには各ケースの独立した証拠が必要。ファイル版のみ、CLI成功、モデル自己申告の3反例はすべてunverified。資料のチェック結果だけで受入を昇格しない。

## 共通記録（CX01、事前確認反映）

```yaml
case_id: CX01
runtime: codex-vscode
surface_evidence: local-preflight-20261111 # scoped native metadata + user Running Extensions report
extension_version: 26.51021.22027
engine_version: 0.164.0-alpha.11.1
superpowers_revision: 7.0.1 # active bundle manifest; upstream commit and identity unknown
routing_revision: 2026-11-11.2
catalog_revision: 2026-11-11.codex.1
billing_contract: chatgpt-plus-bucket-unknown
model_requested: unknown
model_effective: unknown
effort_requested: unknown
effort_effective: unknown
context_requested: default
context_effective: unknown
estimate_cost: {value: unknown, unit: unknown}
actual_cost: {value: unknown, unit: unknown}
usage: {value: unknown, source: unavailable, attribution: unknown}
evidence: unavailable
status: unverified
```

## 参照取得のRED（変更前）

固定評価設定: `gpt-6.1-sol / medium / fork_turns none`。当該チャットの許可リストで要求可能な値。実効モデル・推論・容量・実費はunknown。料金による最安選択ではなく参照取得の評価条件固定。モデル選択のための追加派遣は行っていない。

子は既存資料だけを読み、次の4ギャップを報告した。これは誤った価格流用をしたという結果ではなく、Codex用参照が不足しているというRED。

1. `Codex VS Code adapter and independent-entry loading: unsupported; retrieval gap.`
2. `Applicable Codex ChatGPT subscription price table: absent; retrieval gap.` Copilot換算は既存資料でも禁止されていた。
3. `Two explicitly fixed-model, read-only routing probes ... not expressly supported for Codex by these documents.`
4. `Recording: partially supported; Codex-specific schema absent.`

変更後も同じ問い・評価設定で参照取得を確認する。単一sampleであり、一般的な遵守率や拡張実機成功を示さない。

## 進捗

Task 1～3: 準備済み。Task 4は事前確認を受領して入口・スキル読込を確認し、同一windowの2モデル利用可否をユーザーに確認した。次は当該拡張会話での固定モデル能力試験。Task 5の実機解除は未実施。試験配置は保持している。

## 参照取得の変更後sample

同じ問いと評価設定で、Codexアダプター、条件付き料金表、固定モデル試験と費用保留の区別、Codex記録形式を取得できた。評価者は「入口の本文・所有記録・単独手順書がスキル資料だけには含まれない」点を残るギャップとして挙げた。この指摘を受け、同じ原本配布から入口と手順書を取得すること、試験配置ではAGENTS.md・model-routing-setup.md・.routing-local/ownership.jsonへ辿ることをガイドに明記し、実際に別配布した。変更後sampleを完全合格や実機遵守の証拠としては扱わない。

## ローカル確認の証拠

- 開始コミットと現版のCopilotカタログ／プロファイルのJSONを `json.loads` で比較: 等値。
- 既存F01～F05をDecimalで再計算: 全5件一致。
- overlay・導入手順・新仕様／計画／ケース／検証記録・README・互換性の16資料: 相対リンクと空白を確認。
- CX01～CX10が仕様・ケース・台帳で網羅されること、初回準備時点では全実機行がunverifiedであることを確認。その後CX02だけを証拠に基づきpassへ更新。
- 試験配置: 全8スキルファイルのバイト一致、相対リンク、入口binding、単独手順書、fixtureを確認。案内等を含む所有配布物14件のSHA-256をローカル所有記録へ保存。
- 使い捨てコピーで全14配布物の同版更新、1所有ファイルのドリフトで操作保留、未所有の日本語CRLFファイル保持、所有物だけの解除、ローカル結果記録の保持を再現。
- `git diff --check`: 問題なし。確認用PythonはGit対象外の当該計画workspaceで使い捨て実行し、製品コード／インストーラー／常設テスト基盤は追加していない。

上記は資料・バイト操作の確認であり、拡張の読込、モデル実適用、使用量や節約額を確認した結果ではない。

## 再開に必要な操作

初回の新規会話による事前確認は完了した。次は同じ拡張会話へ `.routing-local/ROUTING-TEST-PROMPT.txt` の依頼を渡す。結果は `.routing-local/routing-results.md` と対応する子thread／turnのネイティブ記録へ保存する。依頼文は利用可否・容量・正式経路・既実施回数を再確認し、前提が変わっていれば該当呼出しだけ保留する。

終了をこの管理側会話で知らせてもらえば、保存結果と対応する実ログを読み取る。個人設定の変更、CLIの代理推論、観測だけの再呼出しはしない。試験終了後に所有物だけ解除し、別の新しい拡張会話で非読込を確認する。

## 受領した拡張事前確認の証拠（2026-11-11）

ローカルの `.routing-local/preflight.md` のSHA-256は `258c3a8a8e4bb841bf00393105462d21fce799b53e4cb807cea5eaa3b4a805b6`。個人絶対パス・認証情報を共有記録へ転記せず、ローカルの `preflight-verification.json` に対応関係を保存した。

- 対象rolloutの1行目は `originator=codex_vscode`、`source=vscode`、試験cwd一致、エンジン版一致。CLIの代理推論ではない。
- 9行目の `world_state.state.agents_md` は試験フォルダと一致し、取り込まれたtextのバイト列が現AGENTS.mdと完全一致。入口SHA-256は `77dae52fc97a0c47e8688553c943eceb341105b06fa5d3657f7297eb3c020d5f`。
- 33／39行目のread callと36／41行目の同call IDのoutputで、配置SKILL・Codex資料・手順書の実読込とexit 0を確認。名前の一覧掲載だけでpassにしていない。
- 所有14ファイルのハッシュ、全8スキル資料と管理原本のバイト一致を再確認。確認対象ソースでは同名model-selectionはこの配置1件。有効Superpowersは7.0.1のソースであることをhost-skills対応とmanifestで確認。管理者ソースの未観測範囲はunknown。
- native toolは `collaboration.spawn_agent`、`model`／`reasoning_effort` と `fork_turns="none"` の経路が公開されている。7.0.1の参照にあるfull-history override可の説明は現行ツール定義と矛盾するため採用しない。本体は編集しない。
- ユーザーが同じwindowのモデル一覧でSol 6.1とLunaの選択可、Running Extensionsで拡張版を確認。正確なIDとの照合は当該ツールとキャッシュで行う。UIの選択可は試験候補の根拠で、子の成功・実効IDは各試行後に別確認する。
- 試験条件は `fixed-capability-probe`、Sol `gpt-6.1-sol` とLuna `gpt-6-luna`、両方medium／fork none、各1回・同時1子。新しいforkと短いbriefの予測ピークを64,000 tokensと置き、当日キャッシュの小さい公称容量272,000の80%＝217,600以内で計画する。これは強制上限ではなく、子の実効容量・最大出力は実行後も観測できなければunknown。
- rate limitのPlus表示と追加クレジットなしの観測、response単位のtoken_usage_recordの存在を確認。子の記録はまだない。集約残枠を子の費用へ帰属させず、actual_costと課金bucketはunknownのまま。

[公式App Server文書](https://learn.chatgpt.com/docs/app-server)はmodel/listを候補と推論設定の取得に用いると説明する。ただし一覧やUIの掲載を完了した推論の証拠とは扱わない。設定によってはbundled catalogを返すことも[公式の別provider構成の説明](https://developers.openai.com/siwc/token-sharing-open-source/codex-app-server)に記載されており、その構成を現在の拡張が使っているとは推測しない。

## 全体レビューと実装判断

実装スナップショット `215efa0` に対し、新しいコンテキストの担当が仕様・計画・全差分と実際の試験配置を読み取りレビューした。要求設定は評価と同じ `gpt-6.1-sol / medium / fork_turns none`。実効設定・実費はunknownで、最安モデルを選んだとは主張しない。通常リスクの資料確認と過去の参照取得結果を適性根拠とし、高リスク向けの能力を理由なく固定要求しなかった。

レビュー結果: 実装範囲のCritical／Important／Minor指摘なし。事前確認の開始に支障なし。Copilotデータ・算術、16資料の参照、14配布物の所有ハッシュ、8スキル資料と単独手順書の原本一致、入口とfixtureを独立確認した。これは拡張受入の合格判定ではない。

レビューで判定対象外にしたものは、CX01～CX10の実機結果、解除後の新会話での非読込、評価者の非公開実効モデル・設定・実費。いずれも証拠待ちの未検証として維持し、成功表示へ含めない判断を管理側で確認した。

実装時の判断とその影響:

1. 管理worktreeも既存運用に合わせて一時領域に配置した。元mainを保全できるが、一時領域が消えた場合は作業ブランチから再作成が必要。
2. 今回は参照案内の適用なので、対照・変更後各1sampleを使い、5反復の文言実験は行わなかった。参照の取得確認に限定し、行動の一貫性は未検証。
3. 操作を具体化するため、試験フォルダへ案内・貼り付け用依頼文・Git対象外の所有記録を追加した。所有配布物として照合・解除できる。
4. 実機試験前の解除は行わず試験配置を保持した。一時ファイルは残るが、次の会話で同じ試験を再開できる。
5. 全体レビューは完成した資料と配置だけを対象とした。実機試験と実費の不足はレビューで補えず、対応完了とは報告しない。

検証用チェックで一度、Gitルートの親階層指定を誤り資料チェック前に失敗した。原因を修正して全チェックを再実行し、上記の成功出力を確認した。製品の失敗を成功に読み替えたものではない。
