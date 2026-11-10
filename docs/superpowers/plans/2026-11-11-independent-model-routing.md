# Independent Model Routing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 既存のモデル選択を独立したMarkdown群へ抽出し、無改変のSuperpowersと組み合わせて導入・検証・解除できる構成へ移行する。

**Architecture:** 本体は別途導入し、このリポジトリでは`overlay/`の共通入口・選択スキル・環境別ガイドを管理する。親の共通指示から選択ルールを読み、正式なモデル指定機能へ接続する。本体と独自レイヤーの版・読込・適用確認を別々に記録する。

**Tech Stack:** Markdown、既存のAgent Skills／custom agents、ネイティブ派遣機能。整合性・算術確認はPython標準ライブラリとGitの読み取りコマンドで行い、実行時コードや依存パッケージを追加しない。

**Spec:** [Superpowers 本体から独立したモデルルーティング仕様](../specs/2026-11-11-independent-model-routing-design.md)

**Status:** Native方式による文書実装・ローカル検証・最終レビュー修正を完了。Task 7の実機Step 2～4は対象環境がなく未実施。実機受入と全体移行完了は未検証。仕様書作成時の承認前という表示は作成時点の履歴で、ユーザーの実装計画作成・Native実装依頼を受けて進めている。

## Global Constraints

- 「公式スキル本体を無改変で利用し、独自のモデル選択を別管理の Markdown 群から適用する。」
- 「能力を先に満たして予想総コストを比較する方針、料金計算、コンテキスト、例外処理、評価条件固定の要件は引き継ぐ。」
- 「初版は手動で再現可能な配置手順を必須とする。インストーラーや更新スクリプトの実装は必須にしない。」
- 「全プロジェクト向けの個人配置を標準とし、プロジェクト内配置を代替手順として示す。」
- 「通常コンテキストを標準とし、予測ピークに上限の20%の余裕を確保する。」料金の鮮度目安は30日、代表候補は最大3件。
- 「修正回数・担当の再開／交代等は対象本体のワークフローに従う。基準版での最大5回を節約目的で緩和しない。」
- 「実験の評価モデル・設定は variant 間で固定する。費用比較のために評価条件を変えない。」
- 「子には作業 brief、必要な成果物、プロジェクト規約、適用設定だけを渡す。」選択のためだけの追加派遣をしない。
- 本体のSubmodule化、自動更新・自動パッチ、独自API・MCP・CLI子セッションラッパー、親モデルの自動変更、請求上限・節約率保証は作らない。
- 候補モデル・料金・プロファイルは移設に合わせて再設計しない。期限切れや拒否が発生した場合だけ既存の更新手順で扱い、構造変更と区別して記録する。
- `pyproject.toml`、`src/`、既存の実行スクリプト、ユーザーの全体設定・MCP設定は変更しない。導入作業では合意した対象の追加部分だけを扱う。
- 実行開始時に`using-git-worktrees`で隔離を確認する。変更前コミットと作業差分を保存し、既存のユーザー変更を削除しない。自動push・公開はしない。
- 将来の委譲では`model-selection`を適用し、必要能力・実行環境・価格・許可IDから選ぶ。最終レビューも同じ手順を使い、評価実験は指定条件を固定する。
- 実機アクセスがなければ文書実装まで進め、実機受入と全体移行完了を未検証として残す。旧記録の成功を新構成に転記しない。

## Review Focus

| 入力・条件 | 期待する動作 | 担当する確認 |
| --- | --- | --- |
| 上流の最高能力指定と安価な適格候補が競合 | 原本を変えずに比較する。従わなければ不適合とする | Task 4のS01・S02 |
| 旧改変版が別スコープに残り、会話に混入 | ソースを特定し、同名重複を除去して新会話で再確認 | Task 5のD04、Task 7 |
| Markdownリンクは存在するが読まれない | 入口を実体として取り込み、スキル読込を独立確認 | Task 3のD01、Task 7 |
| 利用者の既存指示・導入後編集・パス中の空白 | 自分の所有部分だけ展開・解除し、衝突を上書きしない | Task 6のD02・D03 |
| モデル指定済みだが実効不明／Autoで変更 | 未検証と不適用を区別し、必要な再評価をする | Task 4のS03・S04、Task 7 |

## ファイル構成と共通インターフェース

| ファイル | 責務・担当 |
| --- | --- |
| `docs/audits/2026-11-11-independent-model-routing-migration.md` | 保存コミット、上流出自、26変更・6追加・35項目の対応。Task 1・5 |
| `compatibility/superpowers.md` | 本体原本の識別、3環境の互換性・未検証事項。Task 1・5・7 |
| `overlay/skills/model-selection/SKILL.md`と`references/`の既存4資料 | 既存選択ロジックの管理用原本。Task 2・3 |
| `overlay/skills/model-selection/references/runtimes/vscode.md`・`copilot-cli.md` | 既存2ガイドの移設先。Task 2・3・6 |
| `overlay/instructions/model-routing.md` | 短い共通入口。Task 3 |
| `tests/model-selection/independent-routing-scenarios.md` | 無改変本体との競合と導入・解除の入力／期待値。Task 3・4・6 |
| `tests/model-selection/scenarios.md` | 既存架空データF01～F05。変更しない |
| `docs/superpowers/validation/independent-model-routing.md` | IR01～IR18の証拠と状態、シミュレーション・実機の区別。Task 1以降 |
| `docs/model-routing-setup.md` | 個人／プロジェクト導入、解除、更新、復元の手順。Task 6 |
| `README.md` | 新構成の入口と対応状態。Task 6・7 |
| `.agents/skills/`内の本パッケージ所有ファイル | Task 5で有効なローカルコピーを除去。本体を別途導入する方式に統一 |

Task 2では移設先へコピーして整合性を先に確認する。移行前のコピーはTask 5まで一時的に保持するが、検証環境には両方を配布しない。最終構成ではこのリポジトリに本体のスキルコピーを残さず、Git履歴から取得できる状態にする。外部の公式原本は変更しない。

- カタログと7プロファイルのJSONキーは既存のまま使う。記録は仕様§11の`task`、`runtime`、`routing_revision`、`catalog_revision`、`superpowers_revision`、`runtime_version`、`profile`、`model_requested`、`model_effective`、`effort_requested`、`effort_effective`、`context_requested`、`context_effective`、`estimate_credits`、`reason`、`actual_cost`、`evidence`を正式名とする。
- ルーティング初版は`routing_revision: 2026-11-11.1`を使う。これは料金表の版とは別。方針内容を変更したら版を更新する。
- 検証行は`case_id`、`acceptance_ids`、`variant`、`sample`、`runtime`、`runtime_version`、`superpowers_revision`、`routing_revision`、`model_requested`、`model_effective`、`prompt_source`、`expected`、`observed`、`evidence`、`status: pass | fail | unverified`を持つ。`unknown`や`unavailable`を空欄にしない。
- 互換性の表示状態は仕様の「未検証」「文書・シミュレーション確認済み」「実機確認済み」「不適合」。検証行のpassと全体の実機確認済みを同一視しない。
- 全タスクの依存は`1 → 2 → 3 → 4 → 5 → 6 → 7`。上流取得が不能な場合でもTask 2・3・6の文書整備は進められるが、原本依存のTask 4・5と実機判定を完了扱いにしない。

---

### Task 1: 移行台帳と上流原本の確認

**Files:**
- Create: `docs/audits/2026-11-11-independent-model-routing-migration.md`
- Create: `compatibility/superpowers.md`
- Create: `docs/superpowers/validation/independent-model-routing.md`
- Read: `docs/audits/2026-11-05-model-selection-audit.md`

**Interfaces:** Consumes: 開始時HEAD、`a8ea8fe`、`4792d5e`、既存走査。Produces: 保存コミット・所有ファイル一覧、公式URLと検証用の完全なコミットID・一時原本配置・ハッシュ、35項目の対応表、IR01～IR18の未実施行。

- [x] **Step 1: 変更前の状態と移行対象を記録する。** `git status --short`、`git rev-parse HEAD`、`git diff a8ea8fe 4792d5e --name-status -- .agents/skills`、`git ls-files .agents/skills`を実行。保存コミットとユーザー差分の退避先を記録し、26件のM／6件のAを確認する。新しい差分があれば固定件数に合わせて消さず、別行で扱う。
- [x] **Step 2: 公式原本を別の一時ディレクトリへ取得して照合する。** 公式`https://github.com/obra/superpowers`のリリースと初期取込の各ファイルを比較し、対応版が見つかれば全80ファイルを内容と実行権限で照合する。完全一致しなければ差分と取得元を記録し、検証対象として選んだ公式リリースの完全なコミットIDを固定する。最新という理由だけで選ばず、既存のワークフローとの変更点を読む。本体をこのリポジトリへ同梱しない。
- [x] **Step 3: 台帳と互換性の初期記録を作る。** 26変更・6追加の全パスと移行先、走査A01～A20／B01～B03／C01～C12の分類を記載する。分類は本体の役割要件、共通入口のモデル方針、対象外のいずれかで、上流原本の場所と理由を付ける。3環境の版・モデル・適用確認は確認前の値を`unknown`にする。
- [x] **Step 4: 調査結果を確認する。** `git diff --check`を実行し、台帳の差分行32件と走査ID35件の重複・欠落を照合する。原本取得不能ならIR01は`unverified`、理由と再開手順を残す。`a8ea8fe`への復元を原本確認の代用にしない。
- [x] **Step 5: このタスクの3文書だけをコミットする。** 件名: `docs: inventory independent routing migration and upstream provenance`。

### Task 2: 既存の選択スキルと環境別ガイドを抽出

**Files:**
- Create: `overlay/skills/model-selection/SKILL.md`
- Create: `overlay/skills/model-selection/references/{copilot-model-catalog,task-profiles,decision-record}.md`
- Create: `overlay/skills/model-selection/references/runtimes/{vscode,copilot-cli}.md`
- Modify: `docs/superpowers/validation/independent-model-routing.md`

**Interfaces:** Consumes: Task 1の保存コミット内の独自6ファイル。Produces: 内容を維持した管理用原本6ファイル。Task 3は価格・能力を再定義せずこの原本を使う。

- [x] **Step 1: 6ファイルをコピーする。** 元の`model-selection/`4ファイルは同じ相対構造、`using-superpowers/references/vscode-tools.md`と`copilot-cli-tools.md`は上記`runtimes/`へ配置する。元ファイルの削除はTask 5で行う。
- [x] **Step 2: 接続だけを調整する。** `SKILL.md`から環境別資料を選べるリンクを追加し、ガイド内の旧配置依存を移設先の相対リンクへ変更する。価格・候補・能力・例外ルールは維持する。JSON部分を`json.loads`で読み、保存コミットの元データと等値比較する。
- [x] **Step 3: 既存の架空データを再計算する。** 以下をリポジトリルートで実行し、F01～F05が一致することを確認する。これはLLMルーターの実装ではなく固定データの算術確認である。

```bash
python3 - <<'PY'
from decimal import Decimal as D
from pathlib import Path
import json, re
s = Path('tests/model-selection/scenarios.md').read_text()
d = json.loads(re.search(r'```json\s*(.*?)```', s, re.S).group(1), parse_float=D)
for n, out, a_expected, b_expected in d['cases']:
    a = d['models']['fixture-a']
    ai, ao = a['long'] if n > a['threshold'] else a['normal']
    bi, bo = d['models']['fixture-b']['normal']
    assert (D(n)*ai + D(out)*ao)/D(1000000) == D(a_expected)
    assert (D(n)*bi + D(out)*bo)/D(1000000) == D(b_expected)
c = d['models']['fixture-cache']
usd = (D(10000)*c['input'] + D(20000)*c['cached_input'] + D(10000)*c['cache_write'] + D(1000)*c['output'])/D(1000000)
assert usd == D('0.053') and usd/D('0.01') == D('5.3')
print('PASS F01-F05')
PY
```

- [x] **Step 4: 原本と配置済みコピーのリンクを確認する。** Markdown内のローカル参照をファイルの親から解決し、6ファイルを一時的な`.github/skills/model-selection/`へまとめてコピーした場合もリンクが成立することを確認する。`git diff --check`も実行し、結果を記録する。
- [x] **Step 5: 抽出した6ファイルと検証記録をコミットする。** 件名: `refactor: extract model selection into independent overlay`。

### Task 3: 共通入口と配置先に依存しない読込契約

**Files:**
- Create: `overlay/instructions/model-routing.md`
- Create: `tests/model-selection/independent-routing-scenarios.md`
- Modify: `overlay/skills/model-selection/{SKILL.md,references/decision-record.md,references/runtimes/vscode.md,references/runtimes/copilot-cli.md}`
- Modify: `docs/superpowers/validation/independent-model-routing.md`

**Interfaces:** Consumes: Task 2の選択原本。Produces: 方針版`2026-11-11.1`、仕様§7の6規則、仕様§11の記録フィールド、導入時に配置済みスキルの所在を確定させる契約。

- [x] **Step 1: 読込ケースD01を先に定義する。** 「リンクだけ渡す」「入口の6規則を実体として渡す」「配置済みスキルが見つからない」を入力にする。期待値は順に読込証拠なし、明示読込後に選択、所在確認まで当該派遣保留。ファイルの存在だけで読込済みとしない。
- [x] **Step 2: 入口を書く。** 仕様§7の6規則を短い本文にし、`routing_revision`を付ける。導入先で確定したスキルの読込先を入口の末尾に一行追加する方式を定める。管理用原本へユーザー絶対パスを埋め込まず、未解決の配置先を含む入口を使用可能としない。料金表・候補・本体の作業手順を複製しない。
- [x] **Step 3: 選択原本とガイドを接続する。** 記録に不足している版・実効推論フィールドを追加する。新規／再派遣／再開時の再評価、ユーザー指定、評価条件固定、指定不能、実効不明、不一致の区別を確認する。LocalとCopilot、CLIの正式フィールドを実装時の公式資料とライブスキーマで照合し、確認できない設定は雛形のまま残す。
- [x] **Step 4: D01と静的整合性を確認する。** 入口のみで最低限の規則が伝わり、詳細は配置済みスキルから読めることを一時配置で確認する。料金表のJSONがTask 2から不変であること、記録項目が仕様§11と一致すること、リンクと`git diff --check`を確認する。実際の取り込み履歴がなければD01の実機部分は未検証。
- [x] **Step 5: 入口・接続・ケース・記録をコミットする。** 件名: `feat: define independent model routing entry and runtime contract`。

### Task 4: 無改変の上流指示との競合を検証

**Files:**
- Modify: `tests/model-selection/independent-routing-scenarios.md`
- Modify: `docs/superpowers/validation/independent-model-routing.md`
- Modify if needed: `overlay/instructions/model-routing.md`と接続先の独自資料のみ

**Interfaces:** Consumes: Task 1の無改変原本、Task 3の入口、既存架空データ。Produces: 同じ上流原文に入口を追加した場合の判断比較。対象本体を編集した結果は証拠にしない。

- [x] **Step 1: 次の入力と必須判断をケースとして固定する。** 文言の完全一致では採点しない。架空IDを実際に派遣しない。

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

- [x] **Step 2: 変更前と入口追加後を独立コンテキストで比較する。** `writing-skills`に従い、同一の評価モデル・設定・固定データを使う。S01～S04は各variantを5回、S05～S09は代表入力1回から始め、失敗時だけ対象を広げる。変更前は無改変上流のみ、変更後は同じ原文と独自入口・参照資料を渡す。旧カスタム版や本会話の指示を対照入力へ混入させない。
- [x] **Step 3: 観測に基づいて独自レイヤーだけを修正する。** 実際に出なかった変更前の失敗を作らず、既に正しい判断は回帰証拠にする。修正後は影響ケースだけ再確認する。上流の最高能力指示を削って成功扱いにしない。解消不能なら不適合とし、パッチは自動追加しない。
- [x] **Step 4: 判断結果と証拠を記録する。** 上流コミット、入力資料、variant、sample、要求評価設定、観測可能な実効値、各判断を記載する。シミュレーションは実機受入ではない。実行手段や適格な評価設定がなければ未実施の入力と理由を残す。
- [x] **Step 5: ケースと観測記録、必要な独自修正をコミットする。** 件名: `test: document unchanged-upstream routing compatibility scenarios`。

### Task 5: 旧ローカル本体を有効配置から外す

**Files:**
- Remove: Task 1で特定した`.agents/skills/`の本パッケージ所有ファイルのみ
- Modify: `docs/audits/2026-11-11-independent-model-routing-migration.md`
- Modify: `compatibility/superpowers.md`
- Modify: `tests/model-selection/independent-routing-scenarios.md`
- Modify: `docs/superpowers/validation/independent-model-routing.md`

**Interfaces:** Consumes: 完成した独自7ファイル、所有一覧、取得できた公式原本。Produces: 本体コピーを持たない管理リポジトリ、旧パッケージへ戻す保存コミット、重複確認ケースD04。

- [x] **Step 1: 除去前の条件を確認する。** 公式原本の出自、独自6ファイルの抽出・算術確認、共通入口が揃っていることを確認する。上流原本が取得できない場合は除去を保留する。シミュレーションや実機が未実施ならその状態を引き継ぎ、ソース整理を実機対応完了と説明しない。
- [x] **Step 2: D04を定義する。** 「個人スキルに公式版、プロジェクトに旧改変版」「旧選択スキルと新選択スキル」「公式版と独自model-selectionのみ」を比較する。期待結果は前2件が重複・ソース調査対象、最後が意図した組み合わせ。単一ディレクトリを見ただけで全スコープ確認済みにしない。
- [x] **Step 3: 所有一覧と一致する旧ファイルを除去する。** 各ファイルに開始後の利用者変更がないことを確認してから、明示的なパス一覧で`git rm`する。`.agents/`全体や利用先の個人ディレクトリを一括削除しない。本体を上流へ戻して同じ場所に再同梱する方法は採らない。履歴上の原本・旧改変版はGitで取得できる。
- [x] **Step 4: 有効配置と参照を確認する。** `git ls-files .agents/skills`に本パッケージ所有ファイルが残らず、独自7ファイルが存在することを確認する。過去文書の旧パスは歴史的参照として維持し、現在の運用文書・overlayのリンク切れは解消する。35項目の移行分類と原本の役割要件を再照合する。
- [x] **Step 5: 削除対象一覧と台帳・記録をコミットする。** 件名: `refactor: retire bundled custom Superpowers skills`。以後の実行では、削除したスキルを作業ディレクトリから読み直すと仮定せず、外部の公式スキルと作業開始時に保存した計画を利用する。

### Task 6: 個人／プロジェクト導入・更新・解除手順

**Files:**
- Create: `docs/model-routing-setup.md`
- Modify: `README.md`
- Modify: `overlay/skills/model-selection/references/runtimes/{vscode,copilot-cli}.md`
- Modify: `tests/model-selection/independent-routing-scenarios.md`
- Modify: `docs/superpowers/validation/independent-model-routing.md`

**Interfaces:** Consumes: 独自7ファイル、方針版、Task 1の公式原本情報。Produces: 手動で再現できる3環境の導入と解除、ローカル導入記録、D02・D03の結果。実装時にユーザーPCへ無条件で展開する指示ではない。

- [x] **Step 1: 既存内容を含む導入ケースを定義する。** D02は日本語・CRLF・空白を含むパスの既存指示と、2回の同じ導入・更新・解除。D03は管理ブロックまたは配置済みスキルを導入後に利用者が編集。D02では既存部分のバイト列保持と重複なし、解除後の復元、D03では差分説明と上書き保留を期待値にする。
- [x] **Step 2: 共通の手動展開契約を書く。** 入口の管理範囲は`<!-- independent-model-routing:begin -->`から`<!-- independent-model-routing:end -->`まで。開始前に原本の方針版、展開先、既存バイト列の保存先、スキルコピーのファイル別SHA-256、管理部分のSHA-256をローカル導入記録へ保存する。入口本文と確定したスキル読込先を管理範囲に展開し、既存内容・改行を保持する。解除・更新では前回記録と照合し、ドリフトがあれば自動上書きしない。共有文書に個人の絶対パス・アカウント情報を転記しない。
- [x] **Step 3: 環境別の具体手順を書く。** LocalはAgent Customizationsのユーザープロファイル指示、Copilot／CLIは個人用共通指示へ展開し、スキルは`~/.copilot/skills/model-selection/`へ同じ版で配置する。プロジェクト方式は対応する`.github/copilot-instructions.md`または`AGENTS.md`と`.github/skills/model-selection/`。認識先・設定を当該版の公式資料で確認し、既存の同名スキルがある場合は由来を調べる。未所有のコピーを上書きしない。
- [x] **Step 4: 必要な場合だけworkerの手順を示す。** 呼び出し単位指定がない環境に限り、`independent-routing-<runtime>-<model-key>`の固有名で確認済みIDのworkerを登録する。読取用と編集・テスト用の権限を区別し、子の再委譲を防ぐ。CLIへ必要規約を渡す方法とモデル適用を確認する。未確認ID・フィールドの雛形は有効化しない。
- [x] **Step 5: 一時ディレクトリでD02・D03を手順どおり試す。** 実ユーザーファイルを使わず、原本ハッシュ一致、既存部分不変、重複なし、解除後の復元、ドリフト検出を確認する。手順に曖昧さがあれば手順を直して再確認する。製品コードやインストーラーのための新しいテストスイートを作らない。
- [x] **Step 6: READMEを新構成に合わせる。** 公式本体の別途導入、入口と手順書へのリンク、実機未検証、旧方式からの切替・戻し方を明記する。更新後は版を再確認し、影響ケースを再実行する。パッチが必要なら再現ケースを別の構成変更として記録する。
- [x] **Step 7: 手順書・README・ガイド・ケース・記録をコミットする。** 件名: `docs: describe independent routing setup update and rollback`。

### Task 7: 対象環境での受入と最終照合

**Files:**
- Modify: `compatibility/superpowers.md`
- Modify: `docs/superpowers/validation/independent-model-routing.md`
- Modify if confirmed: `README.md`、`overlay/skills/model-selection/references/runtimes/{vscode,copilot-cli}.md`

**Interfaces:** Consumes: Task 6の再現可能な手順、Task 4の入力、仕様IR01～IR18。Produces: 版と環境を限定した対応表、未検証項目と再開手順。3環境が揃った場合のみ全体移行完了。

- [x] **Step 1: 利用可能な実機と権限を確認する。** VS Code Local、VS Code Copilot、CLIを別々に識別し、版・本体ソース・アカウントで許可された正確なID・容量を記録する。利用先への設定追加は認められた対象だけで行い、未依頼の全体設定・親モデルを変更しない。実機がない場合は以下の実機手順を未実施として残し、Step 5以降の文書照合へ進む。
- [ ] **Step 2: 新しい会話で読み込みと2モデルの適用を確認する。** D01・D04により共通指示・選択スキル・本体の読込ソースを確認。同じ短い読取依頼を、利用確認済みの2モデルへ正式指定し、独立した実効メタデータを記録する。実効IDを観測できなければIR08は未検証。表示名や自己申告で補完しない。
- [ ] **Step 3: 実機の競合と例外を確認する。** S01・S02は実際の2モデルについて適性・当該契約の料金を確認して行い、架空IDを使用しない。S03・S04のAuto・拒否・上書き・未対応設定は再現できる範囲を確認する。再現不能の項目は未検証とし、実機で不適用が判明した環境を対応済みとしない。
- [ ] **Step 4: 更新・解除と戻し方を確認する。** 自分の導入部分だけを解除・復元し、他の指示と公式本体が維持されることを確認する。本体の版変更を互換性未検証として再評価する手順を確認する。利用者の導入後編集がある場合はD03に従う。
- [x] **Step 5: 全受入条件を照合する。** 下表と仕様IR01～IR18を一行ずつ確認し、証拠・状態・残課題を書く。文書／シミュレーション／実機を分離し、未実施をpassにしない。旧ACの算術・適性・例外もTask 2・4の証拠に関連付ける。
- [x] **Step 6: 全体レビューを行う。** `requesting-code-review`と`model-selection`に従い、実際の変更、仕様、計画、台帳、検証記録をレビュー対象にする。無改変本体の保持、個人設定の保全、未知値、未検証の表示を重点確認する。評価用の料金表をレビュー担当へ丸ごと配布しない。実装で判断した重要な事項と未検証範囲は最終報告にも残す。
- [x] **Step 7: 検証後の状態をコミットする。** `git diff --check`と現行運用文書・overlayのローカルリンクを確認し、レビューで修正した場合は影響確認を再実行する。件名: `docs: record independent routing acceptance and remaining runtime checks`。実機が未確認なら「文書実装完了・実機受入未検証」と報告する。

## 受入条件とタスクの対応

| 仕様ID | 主担当 | 必要な確認 |
| --- | --- | --- |
| IR01 | 1・5 | 上流出自、無改変原本、旧コピー除去 |
| IR02 | 1・2・5 | 26変更・6追加・35項目、抽出、参照 |
| IR03 | 3・7 | D01、取り込みと明示読込 |
| IR04 | 5・7 | D04、全スコープのソース |
| IR05 | 4・7 | S01、原本を維持した最終レビュー |
| IR06 | 4・7 | S02・S05、領域適性と再評価 |
| IR07 | 2・4 | F01～F05、S08、既存の修正経路入力 |
| IR08 | 7 | 各環境で2つの実IDと独立した実効情報 |
| IR09 | 4・7 | S03・S04、Auto・拒否・変換・不一致 |
| IR10 | 3・4・7 | 80%境界、実効context／effort |
| IR11 | 3・4 | D01・S04、指定不能時の局所保留 |
| IR12 | 1・4・7 | 上流手順照合、S06、レビュー・上限 |
| IR13 | 4・7 | S05・S06、評価固定と役割維持 |
| IR14 | 3・4 | S07、版復元・過剰派遣防止 |
| IR15 | 6・7 | D02・D03、既存内容保全・解除 |
| IR16 | 1・6・7 | 互換性記録、版変更時の再確認 |
| IR17 | 全タスク | 証拠と観測範囲、unknown、状態区分 |
| IR18 | 3・4・7 | S07・S09と既存AC14・AC15、予算・明示指定 |

## 計画の自己レビューと実行時の区切り

- 仕様§1～6はTask 1・2・5、§7～11はTask 3・4・6・7、§12～13はTask 5・6・7、§14の全18条件は上表で担当を定義した。
- 全タスクの入出力、版、記録キー、ケースIDを照合した。Task 2の移行前コピーをTask 5で除去するため、途中の状態を配布完了と扱わない。
- Review Focusの5条件はD01～D04とS01～S04に入力・期待結果を定義し、静的検査だけで実機の読み込み・適用を保証しない。
- 原本取得と実機アクセスの外部依存は分離した。原本がなければ本体除去は保留、実機がなければ個別受入を未検証として残す。独立して進められる文書作業を止めない。
- 計画作成時点では仕様の対応と文書形式だけを確認した。現在の実装・検証状態は上記チェックボックスと[検証記録](../validation/independent-model-routing.md)を参照し、未実施の実機条件と区別する。

本計画はMarkdownを中心とした連続した変更のため、実行方式は親が順番に実装し最後に独立レビューするNativeを推奨する。実機確認が利用できない場合はTask 7の未実施項目と再開手順を渡す。ユーザーがSubagent-drivenを選んだ場合は、各タスクで新規実装担当とレビュー担当を使い、担当へは仕様・対象タスク・必要な依存成果物を渡す。
