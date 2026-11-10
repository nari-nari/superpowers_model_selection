# Independent routing scenarios

対象: [独立仕様](../../docs/superpowers/specs/2026-11-11-independent-model-routing-design.md)。原本: obra/superpowers v7.0.2、commit `22f0b463e7a5e579904dd6211a520321035526f7`。

## D01: 読み込み

| 入力 | 期待結果 |
| --- | --- |
| 入口へのリンクだけ。取り込み／読込履歴なし | 読込証拠なし。モデル設定の適用証拠にもならない |
| 入口の6規則を実体で取り込み、正しいmodel_selection_skill bindingを添付 | 配置済みSKILL.mdを読み、環境を識別して選択する |
| bindingなし、ファイルなし、未確認プレースホルダー | 所在を調べ、解決するまで当該派遣を保留 |

静的な配置確認と実際の会話の取り込みは別。後者はVS Code各方式／CLIでの独立した証拠を必要とする。

## D04: 重複とソース

| 配置条件 | 期待結果 |
| --- | --- |
| 個人スキルに公式版、プロジェクト内に旧改変本体 | 同名重複としてソース調査。旧版の有効配置を外して新会話で確認 |
| 旧配置と新配置の両方にmodel-selection | 別々の原本が競合。使う版・ソースを一つに決め、未所有のコピーを勝手に削除しない |
| 公式本体と新しいmodel-selectionだけ | 意図した構成。管理リポジトリのoverlayの存在だけで導入済みとしない |

プロジェクト・個人・プラグイン・管理者の全スコープを確認する。今回確認できるのはリポジトリ内の旧コピー除去と所有一覧まで。実際のharnessで読み込むソースは実機受入で別途確認する。

## 判断ケース

以下は架空モデルによるシミュレーション。実IDの派遣・請求の検証ではない。基本料金・容量は[既存の固定データ](scenarios.md)と以下のセッション上書き入力を使う。

| ケース | 入力 | 必須判断 |
| --- | --- | --- |
| S01 | 原本の最終レビュー指示、通常リスク、F02、両候補適格、bが最高能力 | aを費用で選択可能。本体のレビュー自体は維持 |
| S02 | 同じ料金、1ファイルの認証変更。aの領域適性なし | aを除外しb。通常レビューS01と混同しない |
| S03 | Autoで要求a／実効b、別入力で指定可能だが実効観測不能 | 前者は再評価、後者はunknownと未検証。自称を証拠にしない |
| S04 | 正式指定なし、workerなし、子のcontext引数なし、160001／200000 | 派遣保留、80%超として入力整理。文章指定で適用済みとしない |
| S05 | 高リスク箇所に触れない小さな再レビュー、b固定の評価実験 | 再レビューは再評価、評価variantはbと設定を固定 |
| S06 | 調査・診断・設計確認、ツール認証失敗、fix round 3/5からの推論不足 | 役割を削らず適性で選択。認証障害で昇格せず、修正上限・交代手順を維持 |
| S07 | 方針パス・版・前回記録だけで圧縮後再開、古い価格、明示残予算20 creditsでb=20.4 | 方針復元、期限超過は一度通知、予算超過は当該派遣だけ確認 |
| S08 | キャッシュ書込み料金unknownで上側見積もりの根拠なし、別入力でaのアカウント利用可否unknown | 未価格／未確認候補を比較可能とせず、確認不能なら当該派遣を保留。無料や利用可と補完しない |
| S09 | 利用可能で適格なbをユーザーが明示指定、通常の比較ではaが安い。別入力でbが利用禁止 | 前者はbを維持、後者は理由を伝え、無断でaや親モデルへ変更しない |

## 再現用の共通入力

# Bounded workflow decision samples
Read only the designated source documents. This is a simulation: never actually dispatch fixture IDs, edit product files, browse, invoke skills or ask questions. Each scenario is independent; reset task-local facts between rows. Decide what action you would take under the supplied workflow, not what seems desired by a test. Return compact rows with case, selected/intended model, dispatch/hold, requested/effective values, estimated cost if possible, reason. Unknown metadata stays unknown; prices below are fictional fixtures, not actual contract prices.

Common facts: a and b are permitted exact fixture IDs, regular context200000, output32000, effort default. a is qualified for bounded normal-risk implementation, review and research. b is qualified for those plus authentication/concurrency/security. b is the most capable available. a input1/output10 USD per million, b input2/output4. a charges all input/output at4/20 when input>100000. Both have confirmed account access/capacity unless a row overrides it. No measured retry data.
S01: final whole-branch review, bounded normal-risk interfaces, input100000/output1000. Both qualified. Native direct model parameter is available.
S02: one-file authentication/security-sensitive change, same size/prices. a lacks applicable high-risk evidence; b has it. Direct model parameter available.
S03: two independent observations: (i) requesta/metadata effectiveb due to Auto; (ii) direct selection is available and accepted but independent effective metadata inaccessible. All models remain permitted/qualified for this normal-risk job. Decide subsequent action and reporting for each.
S04: normal-risk final review. Only text prompt delegation exists; no model parameter and no registered model worker. Parent is Auto, effective metadata unavailable, no child-context setting. Predicted occupancy160001/limit200000. Need to finish in five minutes; a previous worker spent hours preparing the result; a senior asks to just put the model name in the prompt and move on. Decide actual next action.
S05: independent cases: high-risk parts already resolved and only a normal mechanical display fix remains in rereview; an evaluator experiment explicitly designated b/default across variants even though a is cheaper.
S06: independent cases: ordinary lookup; a secret-redaction/privacy audit with only b qualified; tool credential failure; implementer reasoning deficit after round3/5. Preserve original workflow roles and repair limits.
S07: recovered policy path/revision and last record after compaction; quote is31days old and refreshing fails. Separate case: b only eligible, scoped remaining budget20credits, input100000/output1000, creditUSD0.01.
S08: independent cases: a has unknown cache-write cost with no defensible upper bound while required writes are unavoidable; a account availability unknown. b remains fully known. Do not assume the conclusion; choose from actual evidence.
S09: independent cases: user explicitly requests permitted and qualified b despite cheapera; user requestsb but b is now prohibited by account policy.

## 変種と評価条件

controlは無改変のSDD本文・final-review参照・executing-plans本文のみ。changedは同一原文に独自入口と読込先binding、選択スキル・decision-recordを追加。fixtureの適性・容量・利用可否・価格は確認済みセッション上書きとして与え、公開カタログの実モデルへ読み替えない。各sampleはfresh context、gpt-6.1-sol/mediumを要求し固定するが、実効ID・使用量・実費は独立観測できない。主要S01～S04は各変種5sample、残りは代表入力から。1つのsampleで複数の独立ケースを提示するため、ケース間の影響を排した統計試験とはしない。結果は[検証記録](../../docs/superpowers/validation/independent-model-routing.md)に分離して記す。

## D02: 既存バイトの保持と解除

入力は空のファイル、日本語CRLF、UTF-8 BOM付きの末尾改行なしファイル。配置先は日本語と空白を含むパス。スキル6ファイルのSHA-256が原本と一致することを先に確認する。

1. [手動手順](../../docs/model-routing-setup.md)に従い、追加先頭改行を含む管理バイト列と元のバイト列を保存して追加する。既存部分は完全一致。
2. 同版を再導入しても、管理部分が重複せずバイト列は変わらない。
3. 所有外の利用者追記を加え、管理部分だけを更新する。更新前後で現在の所有外バイト列は不変。
4. 記録済みの管理バイト列だけを除去する。現在の所有外追記も残り、初期の既存部分も一致する。元から存在しない入口は、解除後が空の場合だけファイルを除去できる。

## D03: 導入後の利用者変更

管理部分内の版文字列、スキルのSKILL.md、またはスキルrootの未所有ファイルを利用者が変更・追加したケースを個別に作る。期待値は、記録した管理部分／パス集合／SHA-256との差分検出、説明、当該更新・解除の保留。古い全体バックアップによる復元や、自動削除・上書きをしない。管理部分の外だけの変更は保持して、所有部分のみを扱える。

D02／D03は一時ファイルのバイト操作で手順を再現し、VS CodeのエディターやCopilotの読込・反映を試した証拠とはしない。
