# Codex拡張の検証記録

日付: 2026-11-11（Asia/Tokyo）。[仕様](../specs/2026-11-11-codex-extension-validation-design.md)・[計画](../plans/2026-11-11-codex-extension-validation.md)はユーザー承認済み。開始コミット `e29b793bd417b7413f01a5171d4e212d90d03279`。

## 実行環境と範囲

管理変更は隔離branch `feat/codex-extension-validation`。元mainと元の未追跡仕様・計画を保持。ChatGPT Plusはユーザー回答とruntimeで確認。Sol／Lunaの各1試行は完了し、要求引数・runtime設定・fixture・子別トークンを確認した。応答単位の課金bucket、provider実効speed・モデル、クレジット実費はunknown。

管理側のこの会話は拡張由来と確認できていない。GUI操作ツールなし。CLIで推論を実行して代理試験にはしない。別のCodex拡張会話から事前確認結果を受領し、そのsession metadataと指示本文を独立照合した。稼働拡張はユーザーのRunning Extensions確認により `26.51021.22027`、当該拡張会話のエンジンは `0.164.0-alpha.11.1`。拡張側の有効Superpowersはmanifest宣言v7.0.1で、上流commit・原本一致・基準v7.0.2との互換性は未検証。

## 受入台帳

| ID | 文書・ローカル確認 | 拡張実機 | 理由／証拠 |
| --- | --- | --- | --- |
| CX01 | 拡張会話の出所と稼働版・エンジン版・本体の有効ソースを確認 | unverified | VS Code稼働版・上流commit・基準版互換性は未取得。後述の部分証拠を保持 |
| CX02 | 所有ハッシュと原本を照合 | pass | native session metadata・world_state・読込call/outputを独立確認。確認した会話と列挙ソースの範囲に限定 |
| CX03 | 正式指定、順次2子実行、runtime設定とfixtureを独立照合 | unverified | 経路・runtime設定・fixtureは確認済み。provider返却モデル／推論は取得できず、全面適用は未検証 |
| CX04 | 要求値・runtime設定・provider値を分離 | unverified | 局所拒否を保証する根拠がなく拒否枝は未実施。Auto／fallback等も未再現 |
| CX05 | 拡張の親による6架空ケースを確認 | pass（判断シミュレーション） | S01/02/05/06/07/08の選択・除外・保留を照合。実作業の品質・一般遵守率は未検証 |
| CX06 | Plusをruntimeでも確認。追加クレジットなしの観測あり | unverified | 含有枠を支持するが応答単位bucket・換算根拠は未取得 |
| CX07 | 子別4応答の帰属・重複除外・生usageを独立照合 | pass（トークン取得の範囲） | 各モデル1標本。クレジット換算・実費・一般的な節約効果はunknown |
| CX08 | 有効7.0.1の役割・独立性・修正上限と紙上判断を比較 | unverified | 比較は確認済み。実際の独立レビュー・修正ループは実行していない |
| CX09 | 所有13ファイル解除・証拠保持・新しい拡張会話を独立照合 | pass | 新threadのagents_md空、host_skillsにmodel-selectionなし。公式Superpowersは残存 |
| CX10 | S07の復元・条件変更・期限超過・予算の紙上判断を照合 | unverified | 実際のcontext圧縮・圧縮後再開は未再現 |

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

今回の実装・上限付き検証作業は終了。Task 1～3の準備、Task 4の2モデル能力試験・架空判断・未知値の処置、Task 5の所有物解除と新会話の非読込確認まで実施・記録した。再試行0、native子は合計2、最大同時1子。未観測のprovider実効・費用と未再現ケースを残しているため、全面的な実機受入完了・常設導入成功・節約額は主張しない。

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

## 終了状態と将来の再開条件

2モデル試験、判断ケース、解除、新会話での非読込確認は終了した。試験フォルダには `.git`、結果を除外する `.gitignore`、ローカル証拠の `.routing-local/` だけを残した。入口・スキル・fixtureは解除済みで、過去の案内は現在の有効配置を指さない。元の所有manifestは履歴として保存し、解除後の存在を期待するmanifestとして再利用しない。

追加の同じ試験やユーザー操作は要求しない。将来、provider返却metadata・含有枠の換算根拠・拒否を安全に再現する経路・基準本体版の互換性証拠が得られた場合に、該当受入だけを再開できる。再配置する場合は現行手順で新しい所有記録と実効ソースを確認し、過去の試験フォルダを導入済みと扱わない。

## 判断・解除・新会話の最終照合（2026-11-11）

ローカル `final-verification.json` に独立照合結果を保存。結果4資料のSHA-256は次の通り。

| 資料 | SHA-256 |
| --- | --- |
| workflow-results.md | d2bf37f3087c1abe7b4fb3bb6f91ddc06f8e381dbe1a7a986d99f1ac69128dfe |
| workflow-evidence-index.json | b191d434ceb98cd30b06b5bbf117a894248779985493f0d446d799dac70a8333 |
| cleanup-results.json | 27d4b2fd08af208b611f91e5b10c58c5e70d989d67325b6c6642d181870decc1 |
| post-removal.md | 90614bf546211d4a6d4330b010b9e4acc7760d06b012612d78cf04549b98017e |

- S01は通常レビューでa、S02は認証適性からbを選択。S05は小再レビューで候補を再評価し、数量未指定なら費用順位を保留、指定evaluatorはbに固定。S06はlookupの数量不足・privacy適性・認証障害・推論不足を分離。S07は方針復元、古い価格の不確実性、架空予算20に対する20.4の保留。S08は未価格／利用可否不明のaを外し、確認済みbを選択。数量不足から勝手に「最安a」を補わない判断を適合とした。
- 実際の7.0.1のSDD・final-review・fix-loop・inline本文と比較し、著者とreviewerの独立性、task修正上限5回・交代条件、最終waveとinline修正passの区別を保持した。これは紙上判断であり、本当の修正・独立review・圧縮を実行した証拠ではない。
- 解除前の全14件照合は成功。除去集合は所有manifestから.gitignoreだけを除いた13件と完全一致し、現物も不在。ドリフト・エラーなし。.gitignoreは結果をGit対象外にするため保持した。
- 解除前後の既存証拠13件は全件ハッシュ一致、.git・.routing-localは保持。追加の子起動はなく、旧親のnative spawnは最初の2件のまま。
- 新しい拡張会話は旧親・2子のいずれとも異なるthreadで、session metadataのsource=vscode／originator=codex_vscode／試験cwdを独立確認。9行目のworld_stateはagents_md={}、初期および最新host_skillsにmodel-selectionなし。host_skills本文のSHA-256は `5f3c71d58fd3a4658926710879539970a628080be5107e8699b94f39728ae3e4`。公式Superpowersは残る。新会話のspawnは0件。

現物の不在だけでなく、新しい会話での実際の非取り込みも確認してCX09をpassとした。確認したソース・版・時点だけの証拠であり、将来の会話や他環境へ一般化しない。未検証欄を成功へ丸めることなく、この検証実施を終了する。

## 2モデル試験の独立照合（2026-11-11）

試験結果のSHA-256は `b31b18b67b2344292d1b9a346a2aa23d86902ae58c9e9607deba9d2cfa0f5fa5`、索引は `b7eb62312006fce9e0c1cb032dea2a16f18bb528d58fb30e5f6aa3c3efa43d0e`。ローカルの `routing-verification.json` に照合結果を保存した。対象2子以外の会話内容・アカウント識別子・個人絶対パスは共有記録へ転記しない。

| 要求モデル | 要求／runtime推論 | 子runtime報告context | fixture | input合計 | output合計 | total合計 |
| --- | --- | ---: | --- | ---: | ---: | ---: |
| gpt-6.1-sol | medium／medium | 475000 | 一致 | 33392 | 48 | 33440 |
| gpt-6-luna | medium／medium | 258400 | 一致 | 32205 | 239 | 32444 |

- 親のnative spawn callは2件のみ。両方とも正式引数のmodel、reasoning_effort=medium、fork_turns=noneを確認。messageは保存ログでは保護された不透明値なので復号せず、同一briefの根拠はnative側のlive呼出し記録と分ける。
- state DBの当該親の `thread_spawn_edges` は索引の2子と一致。子それぞれのsession metadataはcodex_vscode・試験cwdと一致し、turn_contextおよびDBのmodel／reasoning_effortが要求と一致。自称による確認ではない。
- Sol完了後にLunaを起動しており、同時実行なし。各子のtool callはfixture読み取りのexec 1件だけで、最終返答はfixtureの1行と完全一致。編集・ネットワーク・再委譲は観測されなかった。
- 各子2応答・計4件の `token_usage_record` はthread／turnへ帰属し、response_idは全件一意。生フィールドの合計を索引と再計算照合した。cached inputはSol28288／Luna15744、reasoning outputはSol0／Luna142で、input／outputへ重ねて加算していない。
- これらはruntimeの設定・context報告であり、provider返却のmodel／effort／contextではない。native結果の保守的な区別を維持し、model_effective・provider_model・effort_effective・最大出力はunknownとする。runtime値を無条件にprovider実効値へ転記しない。
- 所有14件と保全対象19件のハッシュはすべて一致。設定・本体・fixtureは保持されている。条件付きの拒否枝は、派遣前の局所拒否を保証できないため呼び出さず、未実施と記録した。
- Plusのクレジット実費は算出しない。同じ短い作業でもcache・推論・tool出力条件が異なり、各1標本のトークン差から一般的な節約率・能力優劣を主張しない。

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

この初回レビュー時点で判定対象外にしたものは、CX01～CX10の実機結果、解除後の新会話での非読込、評価者の非公開実効モデル・設定・実費。当時は証拠待ちの未検証として維持した。その後の実機証拠に基づく更新は現在の受入台帳を参照する。

実装時の判断とその影響:

1. 管理worktreeも既存運用に合わせて一時領域に配置した。元mainを保全できるが、一時領域が消えた場合は作業ブランチから再作成が必要。
2. 今回は参照案内の適用なので、対照・変更後各1sampleを使い、5反復の文言実験は行わなかった。参照の取得確認に限定し、行動の一貫性は未検証。
3. 操作を具体化するため、試験フォルダへ案内・貼り付け用依頼文・Git対象外の所有記録を追加した。所有配布物として照合・解除できる。
4. 実機試験前は試験配置を保持して次の会話で再開した。その後、最終確認で所有13ファイルを解除済み。現在は証拠と.gitignore等だけが残り、将来の試験には新しい配置が必要。
5. 全体レビューは完成した資料と配置だけを対象とした。実機試験と実費の不足はレビューで補えず、対応完了とは報告しない。

検証用チェックで一度、Gitルートの親階層指定を誤り資料チェック前に失敗した。原因を修正して全チェックを再実行し、上記の成功出力を確認した。製品の失敗を成功に読み替えたものではない。

## 最終記録の監査と終了

実機結果を追記した最終差分についても、新しいコンテキストの担当が読み取り監査した。要求設定は同じ固定評価条件 `gpt-6.1-sol / medium / fork_turns none`。実効設定・費用はunknownで、Codex拡張の追加試行ではない。重大指摘なし。Minorとして過去の配置保持の記述が現状と紛らわしいと指摘され、履歴時点と現在の解除状態を明記した。再試行や追加の子実行は行わず、資料・証拠の整合性を再確認した。

監査が判定しなかった範囲は、provider実効モデル・推論・context・最大出力、実費・bucket・節約、一般的なworkflow品質と本当の修正／独立review／圧縮、未再現の拒否・fallback等、完全な版互換性・上流同一性、無関係な全体設定や監査へ渡していないrawログ、初回実装修正である。これらを本監査の成功へ含めず、管理側で既に直接照合した対象raw metadataとローカル証拠の範囲を維持する。

今回依頼された実装と限定検証の記録を確定し、変更は `feat/codex-extension-validation` に保存する。既存Copilot3方式の未検証状態はそのまま。全面的な対応完了や価格による最適選択・節約を主張せず、今後の検証に必要な条件を上に残す。
