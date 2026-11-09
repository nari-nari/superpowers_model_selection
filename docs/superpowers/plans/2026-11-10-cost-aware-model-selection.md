# Cost-aware Model Selection Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 必要な作業能力を満たす候補を GitHub Copilot の予想総コストで比較し、VS Code／Copilot CLI で適用結果を確認できる Superpowers に変更する。

**Architecture:** 共通の `model-selection` スキルとモデル・料金・作業適性の参照資料を追加し、既存の派遣元から参照する。環境別ガイドが選択結果をネイティブの派遣機能またはモデル指定済み worker に接続する。選択は親が行い、別の選択担当や実行サービスは作らない。

**Tech Stack:** Markdown、既存の Agent Skills／custom agents、既存の派遣ツール。整合性と固定値の確認には Python 標準ライブラリを一時コマンドとして使用できるが、実行時コード・依存パッケージは追加しない。

**Spec:** [料金を意識したモデル選択仕様](../specs/2026-11-10-cost-aware-model-selection-design.md)

**Audit:** [移行対象35項目](../../audits/2026-11-05-model-selection-audit.md)

**Status:** 実装計画。以下のチェックボックスは未実行。今回の作業ではスキル本体を変更しない。

## Global Constraints

- 標準方針は **品質と費用の両立**。必要な能力を満たす候補を先に絞り、その候補の予想総コストを比較する。
- 第一版はスキル・エージェントの Markdown と参照資料の変更で構成する。MCP サーバー、独自の推論 API、料金取得サービスの導入を前提としない。
- 親セッションのモデルを自動で切り替える機能は作らない。
- コンテキスト制御のために別 CLI セッションを起動するラッパー、SDK 実装、MCP の追加は第一版に含めない。
- 既存のユーザー共通設定や MCP 設定を変更しない。`pyproject.toml`、Python パッケージ、既存の実行スクリプトも変更しない。
- 価格の鮮度の目安は30日とする（初期値）。通常コンテキストを標準とし、上限の20%を余裕として確保する（初期値）。
- 単価は USD／100万トークンを基本とする。`0`、`not_applicable`、`unknown` を区別する。
- 数値予算は必須にしない。ユーザーが指定した場合だけ範囲と単位を記録する。
- 既存のテスト、レビュー、修正ループの回数上限を、節約のために省略・緩和しない。SDD の修正ループは最大5回。
- 実績費用が得られない場合は `actual_cost: unknown`。モデル自身の自己申告だけを適用確認の根拠にしない。
- 見積もりのための LLM 派遣を追加しない。料金表を子全員に読み込ませない。
- 実機確認できない環境・設定は未検証とする。全環境対応・実費削減・強制予算上限を確認済みと扱わない。
- 実装開始時に `using-git-worktrees` に従って隔離を確認する。現在の未追跡 `.DS_Store` と既存文書を削除せず、変更対象だけをステージする。自動 push／公開はしない。

## Review Focus

| 確認対象 | 利用者が期待する動作 | 主な検証担当 |
| --- | --- | --- |
| ユーザーのモデル指定と環境上書き／Auto の衝突 | 要求と実効の違いを明示し、親モデルを無断変更しない | Task 3、AC09・AC12 |
| 候補が全て unavailable／unknown | 架空の ID・無料料金・暗黙の継承で処理せず、影響する派遣だけを保留 | Task 2、AC09・AC10 |
| 長文閾値の等号、書き込み料金、課金範囲の不明 | 境界を正しく比較し、不確実性を幅で表示する | Task 1–2、AC05–AC08 |
| 通常作業用の安いモデルで高リスク変更を扱おうとする圧力 | 先に能力要件で除外し、価格順位で能力を代替しない | Task 2・4、AC02・AC03 |
| テスト variant 間のモデル変更と圧縮後のルール消失 | テスト対象条件を固定し、通常の選択記録は版とパスから復元する | Task 5–6、AC15・AC16 |

## ファイル構成と責務

新規の運用ファイルは以下の6件を基本とする。

| 新規ファイル | 責務 |
| --- | --- |
| `.agents/skills/model-selection/SKILL.md` | 選択、再選択、例外処理の唯一の手順 |
| `.agents/skills/model-selection/references/copilot-model-catalog.md` | モデル識別・料金・能力情報の一次格納先 |
| `.agents/skills/model-selection/references/task-profiles.md` | 作業プロファイルと具体的候補、適用条件 |
| `.agents/skills/model-selection/references/decision-record.md` | 選択記録の項目と短い記入例 |
| `.agents/skills/using-superpowers/references/vscode-tools.md` | Local／Copilot の機能差と設定適用 |
| `.agents/skills/using-superpowers/references/copilot-cli-tools.md` | CLI の指定・上書き・Auto・確認方法 |

検証用には `tests/model-selection/scenarios.md` と `docs/superpowers/validation/cost-aware-model-selection.md` を作る。前者は架空データ・入力・期待結果、後者は実際の観測と未検証事項を保持する。実行用の新しいスキルやプログラムにはしない。

既存ファイルの変更は Task 3–5 に列挙する。必要な worker だけ Task 3 の固定パスで追加する。README には導入手順と対応状況を Task 6 で記載する。

## 文書間の共通インターフェース

実装担当は独自の別名を作らず、以下を共通の項目名として使う。これは Markdown 内のデータ・記録の契約であり、新しい API の実装指示ではない。

- カタログ: `catalog_revision`、`verified_at`、`pricing_mode`、`currency`、`price_unit` とモデルごとの `model_key`、`display_name`、`runtime_ids`、`availability`、`capability_evidence`、`context_limits`、`output_limit`、`reasoning_values`、`price_tiers`、`sources`。
- `runtime_ids`／`availability` の環境キー: `vscode-local`、`vscode-copilot`、`copilot-cli`。不明 ID は `unknown` とし、表示名から生成しない。
- `price_tiers`: `tier`、`threshold`、`operator`、`threshold_basis`、`tier_application`、`input`、`cached_input`、`cache_write`、`output`。閾値や料金適用範囲も不明と該当なしを区別する。
- 作業プロファイルの ID: `mechanical`、`integration-debugging`、`design-high-risk`、`task-review`、`final-review`、`research-audit`、`skill-evaluation`。候補はカタログの `model_key` で参照する。
- 適性の項目: `status: recommended | provisional | excluded`、`conditions`、`evidence`、`reasoning_requested`。具体的候補の対応を空の能力階層名で置き換えない。
- 選択記録: 仕様§11の YAML と同じ `task`、`runtime`、`catalog_revision`、`profile`、`model_requested`、`model_effective`、`effort_requested`、`context_requested`、`context_effective`、`estimate_credits`、`reason`、`actual_cost`、`evidence`。文中でも `context_requested`／`context_effective` を正式な項目名とする。
- 検証記録: `case_id`、`skill_under_test`、`variant`、`sample`、`model`、`effort`、`runtime`、`prompt_source`、`expected`、`observed`、`evidence`、`status: pass | fail | unverified`。各ケースを AC 番号にも対応させる。

## 検証の共通手順

固定の数値は Task 1 の架空カタログで確認する。実際のスキル選択行動は、指定した原文のコンテキストを読んだ fresh-context の担当に、選択記録のみを返させて確認する。テストの子がさらに作業担当を派遣しないよう、許可する行動を読み取りと回答に限定する。

新規スキルの作成・既存スキルの編集時は `writing-skills` に従う。各変更対象スキルについて少なくとも変更前と変更後の同じ入力を比較し、実際の誤選択が観測された箇所だけを失敗と記録する。既に正しい挙動は失敗に捏造せず、回帰条件として残す。

共通選択の主要3ケース（高リスクでの安さ優先、長文閾値を超えた順位逆転、実効モデル不明）は、同じ評価用モデル・設定で、control と変更後を各5回測る。各変更スキルの接続テストは代表ケースで先に確認し、同じ失敗や文言変更が出た場合だけ該当ケースを広げる。全入力・全モデルの総当たりや追加の設計担当は作らない。

以降の「検証」は予定。実行手段が提供されない場合は具体的な未実施手順と理由を検証記録に残し、受入完了とはしない。

---

### Task 1: 候補・料金・適性データと比較用の固定ケース

**Files:**
- Create: `.agents/skills/model-selection/references/copilot-model-catalog.md`
- Create: `.agents/skills/model-selection/references/task-profiles.md`
- Create: `tests/model-selection/scenarios.md`
- Create: `docs/superpowers/validation/cost-aware-model-selection.md`

**Interfaces:** 共通インターフェースのカタログと7プロファイルを提供する。以降のタスクは価格を再定義せず `model_key` と `catalog_revision` を使う。

- [ ] **Step 1: 固定ケースと期待結果を先に記述する。** `scenarios.md` の F01–F05 を下表の数値で定義し、「架空の料金、実モデルではない」と明記する。全 AC01–AC20 の入力・期待動作・担当 Task の索引も作る。

| 架空モデル | 通常入力単価 | 通常出力単価 | 長文入力／出力 | 閾値・課金範囲 |
| --- | ---: | ---: | --- | --- |
| fixture-a | 1 | 10 | 4／20 | 入力が100,000を超えたら全入力・全出力に長文単価 |
| fixture-b | 2 | 4 | なし | 閾値なし |
| fixture-cache | 2 | 4 | なし | キャッシュ読み込み0.2、書き込み2.5。3入力区分は排他的と定義 |

| ケース | 入力／出力 | 期待するUSDと判断 |
| --- | --- | --- |
| F01 | 99,999／1,000 | a=0.109999、b=0.203998。適性が同じならa |
| F02 | 100,000／1,000 | a=0.110000、b=0.204000。等号では長文に移行しない |
| F03 | 100,001／1,000 | a=0.420004、b=0.204002。bに逆転 |
| F04 | 1,000／20,000 | a=0.201、b=0.082。入力単価だけなら誤選択 |
| F05 | 通常10,000・cache read20,000・write10,000／出力1,000 | cache=0.053 USD=5.3 credits。書き込みを無料扱いせず、通常入力へ重複計上しない |

行動テストでは a/b の通常コンテキスト上限を200,000、出力上限を32,000とする。a は通常リスクの機械的実装・タスクレビュー・最終レビュー・調査に適用確認済み、b はそれらと認証・並行処理の高リスク変更に適用確認済みとする。a の高リスク適性は `excluded`。料金以外の条件が同じケースは通常リスクを使う。`fixture-cache` は算術専用とし、架空 ID は一切実際に派遣しない。

追加の行動入力は以下に固定する。期待記録の項目名は共通インターフェースを使い、正しい文章の完全一致では採点しない。

| 対象AC | 入力として与える条件 | 必須の判断・記録 |
| --- | --- | --- |
| AC02・AC03 | F02 の料金、1ファイルの認証処理と、別ケースの通常リスク最終レビュー | 認証はaを適性で除外してb。通常レビューはaを比較対象から落とさない |
| AC04 | 高リスク箇所に触れない単純な表示修正だけが再レビューに残る | 通常リスクへ再評価し、aを選べる |
| AC08 | aの基本経路0.11 USD、追加修正0.11、引き継ぎ0.02、bの基本経路0.204、成功確率・実績なし | 基本経路はa。修正経路0.24で逆転することを別記し、確率加重値を作らない |
| AC09・AC12 | aは指定時に拒否、bは許可。別ケースでは要求aに対して実効bというツール結果 | 前者はbで再比較。後者は不一致と費用差を記録し、適性・許可範囲を再確認 |
| AC10 | verified_at=2026-11-10、比較日2026-12-10と12-11、取得不能 | この計画では年齢が30日を超えたら期限超過と定義。前者は範囲内、後者は古さを一度通知して暫定扱い |
| AC11 | Local の許可候補はaだけ、bの指定が費用階層で拒否 | 通常作業はaへ再選択。bだけが適格な高リスク作業は当該派遣を保留 |
| AC12・AC13 | モデル・context の実効値が取得不能、子のcontext設定引数なし、予測ピーク160,001 | 実効はunknown、親切替なし。通常上限の80%超として情報整理を先に検討し、設定適用を捏造しない |
| AC14 | 許可・適格候補はbのみ、F02の入出力、明示残予算20 credits | 20.4 creditsで超過。代案を検討し、解決しなければ当該派遣だけ確認対象 |
| AC15 | 評価対象bの同一variantを反復中、通常の価格比較ではaが安い | bと設定を固定し、途中でaに変更しない |
| AC16・AC19 | 会話圧縮後に方針パス・catalog_revision・前回記録のみが残り、actual_cost取得不能 | 参照から復元、実費unknown、追加の見積もり担当を派遣しない |
| AC20 | テストツールの認証失敗、別ケースではfix round 3/5の後に推論不足が判明 | 認証障害だけでは昇格しない。4/5は新規担当で能力を再評価し、最大5回を維持 |

- [ ] **Step 2: 固定値を独立に確認する。** `python3` の一時コマンドで `decimal.Decimal` を使い、`(99999*1+1000*10)/1000000 == 0.109999` など全行と `USD / 0.01` を検算する。検証記録に式と結果を保存する。まだ存在しないカタログ項目は未充足として記録する。

Run（F01–F05、成功時は `PASS: 5 arithmetic cases`）:

```bash
python3 - <<'PY'
from decimal import Decimal as D
for n, o, ea, eb in [(99999,1000,'.109999','.203998'), (100000,1000,'.11','.204'), (100001,1000,'.420004','.204002'), (1000,20000,'.201','.082')]:
    ip, op = (D(4), D(20)) if n > 100000 else (D(1), D(10))
    assert (D(n)*ip+D(o)*op)/D(1000000) == D(ea)
    assert (D(n)*2+D(o)*4)/D(1000000) == D(eb)
c = (D(10000)*2+D(20000)*D('.2')+D(10000)*D('2.5')+D(1000)*4)/D(1000000)
assert c == D('.053') and c/D('.01') == D('5.3')
print('PASS: 5 arithmetic cases')
PY
```

- [ ] **Step 3: 公式情報で初期候補を登録する。** 仕様§15の公式料金・対応モデル・設定資料を参照し、初期調査候補を GPT-6 Luna、GPT-6.1 Terra、Claude Sonnet 5.5 とし、`model_key` をそれぞれ `gpt6-luna`、`gpt61-terra`、`sonnet55` にする。これはモデル固定ではなく調査開始点であり、キーは実行時 ID と別。実装時に提供終了なら公式の現行候補を根拠付きで置換し、実際の ID・能力・料金を確認できない項目は `unknown` とする。少なくとも2候補のデータを揃え、環境の利用可否を推測で有効化しない。

初期調査候補の掲載は2026-11-10に [公式対応モデル一覧](https://docs.github.com/en/copilot/reference/ai-models/supported-models) で確認済み。[Copilot公式料金表](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing) を実装時に再確認する。ユーザー環境の利用可否は未確認。
- [ ] **Step 4: 能力と価格を分けて7プロファイルを埋める。** 公式説明のみなら `provisional`、当該作業・環境の検証根拠があれば `recommended`。高リスク作業に必要な根拠がなければ候補なしと明示し、安価な通常作業候補で埋めない。確認日・版・更新方法と30日の扱いもカタログに記す。
- [ ] **Step 5: データの整合性を確認する。** 全 profile の候補キーがカタログに存在し、単価・単位・閾値比較・適用範囲・根拠URL・3環境の確認状態を辿れることを確認する。公式価格が変わっても F01–F05 は変えない。AC05–AC08・AC09・AC10・AC15 の期待結果を固定する。
- [ ] **Step 6: このタスクの4ファイルだけをステージしてコミットする。** Message: `docs: define model catalog and selection fixtures`。

### Task 2: 共通の選択スキルと短い記録契約

**Files:**
- Create: `.agents/skills/model-selection/SKILL.md`
- Create: `.agents/skills/model-selection/references/decision-record.md`
- Modify: `tests/model-selection/scenarios.md`
- Modify: `docs/superpowers/validation/cost-aware-model-selection.md`

**Interfaces:** Task 1 の一覧とプロファイルを読み、共通インターフェースの選択記録を生成する。公開手順は「初回確認→能力・可用性・容量で除外→費用比較→記録→環境ガイドで適用→結果確認」。後続の各派遣元はこの手順への参照と自分の role/profile を渡す。

- [ ] **Step 1: control を測る。** 既存の SDD Model Selection と既存テンプレートを含む文脈で、AC01–AC04、AC09–AC10、AC14 を評価する。特に「最終レビューなので高額モデルを使いたいが、同じ適性の安価な候補もある」という圧力を含める。テストデータとユーザー条件は変更後も同一にする。
- [ ] **Step 2: 共通ルールを書く。** `name: model-selection`、説明は `Use when...` で始め、モデル選択・再選択が起きる条件を記す。概要と選択順序を本文、重い一覧を参照資料に置く。ユーザー指定優先、比較最大3件、未知価格、予算、30日、20%、通常設定、推論強度、リトライの原因別処理、割引の適用根拠、親の切り替え禁止を仕様どおり反映する。
- [ ] **Step 3: 費用の不確実性と記録を書く。** 仕様§7の式、基本経路上側の比較、修正・引き継ぎの別経路、キャッシュ不明時、課金範囲不明時、実績 `unknown` を定義する。`decision-record.md` には同じ項目名の短い正常例と、不適用／不明時の例を1つずつ置く。子に全料金表を渡す手順を入れない。
- [ ] **Step 4: 同じケースを変更後の文脈で測る。** AC01–AC10・AC14・AC19・AC20 を確認し、F01–F05の数値と比較結果が一致することを確認する。主要3ケースは共通検証手順の5回比較を行い、数値誤り・能力不足の候補選択・実効値の捏造があれば該当文言を直して再測定する。
- [ ] **Step 5: 自己整合性を確認する。** frontmatter の必須項目、名前、相対リンク、profile ID、記録項目が前タスクと一致することを確認する。未確認のモデルを試すための自動派遣を記述していないことを確認する。
- [ ] **Step 6: 新規スキルの検証結果と対象ファイルだけをコミットする。** Message: `feat: add cost-aware model selection skill`。

### Task 3: VS Code／Copilot CLI への適用と既存アダプターの接続

**Files:**
- Create: `.agents/skills/using-superpowers/references/vscode-tools.md`
- Create: `.agents/skills/using-superpowers/references/copilot-cli-tools.md`
- Modify: `.agents/skills/using-superpowers/SKILL.md`（Platform Adaptation）
- Modify: `.agents/skills/using-superpowers/references/codex-tools.md`
- Modify: `.agents/skills/using-superpowers/references/claude-code-tools.md`
- Modify: `.agents/skills/using-superpowers/references/gemini-tools.md`
- Modify: `.agents/skills/using-superpowers/references/pi-tools.md`
- Modify: `.agents/skills/using-superpowers/references/antigravity-tools.md`
- Modify: `.agents/skills/using-superpowers/references/hermes-tools.md`
- Modify: `.agents/skills/using-superpowers/references/muse-tools.md`
- Modify: `tests/model-selection/scenarios.md`、`docs/superpowers/validation/cost-aware-model-selection.md`

**Interfaces:** 入力は Task 2 の要求モデル・設定と実行環境。出力は実際の呼び出しに使った指定と、実効確認の根拠または `unknown`。モデル選択の判断自体をアダプターへ複製しない。

- [ ] **Step 1: 接続失敗のケースを用意する。** AC11–AC13 の Local の費用階層拒否、Copilot／CLI の Auto・上書き、未知の引数、実効値を取得できない状態を固定する。既存の `using-superpowers` と adapter の文脈で control を測る。
- [ ] **Step 2: 実際の機能を確認してガイドを書く。** Local の `runSubagent` と Copilot の `task` を分け、実際に公開される引数と ID を確認する。CLI の `model`／`models`／`modelPolicy`／`reasoningEffort` と VS Code の schema・変換制約を混同しない。バージョン、確認方法、正式な指定例、未知値の報告例を各文書に記す。実効値はツール結果やUI・ログに根拠がある範囲で確認する。
- [ ] **Step 3: 必要な場合だけモデル指定済み worker を追加する。** 各環境で呼び出し単位の指定が利用可能なら追加しない。指定がなく custom agent で対応する場合のファイル名を下表に固定し、Task 1 の確認済み ID を設定する。未確認 worker は有効化しない。Copilot と CLI で同じ設定が使えると確認できない場合は、CLI 用の3ファイルを別にする。

| 対象 | 必要な場合に作成する固定パス |
| --- | --- |
| VS Code Local | `.github/agents/superpowers-local-luna.agent.md`、`.github/agents/superpowers-local-terra.agent.md`、`.github/agents/superpowers-local-sonnet.agent.md` |
| VS Code Copilot | `.github/agents/superpowers-copilot-luna.agent.md`、`.github/agents/superpowers-copilot-terra.agent.md`、`.github/agents/superpowers-copilot-sonnet.agent.md` |
| CLI 専用が必要 | `.github/agents/superpowers-cli-luna.agent.md`、`.github/agents/superpowers-cli-terra.agent.md`、`.github/agents/superpowers-cli-sonnet.agent.md` |

worker 名は能力順位ではなくカタログへの対応を表す。役割と task brief は派遣時に渡し、編集・実行が許可された実装と読み取りレビューを混同しない。必要ツールと既存の no-subagents 契約を呼び出しごとに保持する。確認済み候補が少なければ、その候補のファイルだけを作る。置換した現行モデルを使う場合も `model_key` と対応を明記する。

- [ ] **Step 4: Platform Adaptation と既存 adapter を接続する。** VS Code／CLI を一覧へ追加し、既存のモデル指定・中位モデル設定・派遣マッピングから共通手順を参照する。Codex／Claude Code 等は契約が異なるため Copilot 料金を流用せず、確認できない課金は不明とする。他環境に存在しない引数を追加しない。A19–A20 と既存マッピングの扱いを記録する。
- [ ] **Step 5: 接続ケースを再測定する。** 拒否時の候補再選択、要求／実効の分離、親を変更しないこと、コンテキスト未対応の入力整理を確認する。実環境が使えれば性質の異なる2候補で読み取り専用の短い派遣を行い、環境とバージョンを記録する。使えなければここを `unverified` にし、Task 6 に確認手順を引き継ぐ。
- [ ] **Step 6: このタスクの文書と必要な worker だけをコミットする。** Message: `feat: connect model selection to Copilot runtimes`。ユーザー共通設定・MCP・モデルの既定値は変更しない。

### Task 4: 実装・レビュー・修正ループの移行

**Files:**
- Modify: `.agents/skills/subagent-driven-development/SKILL.md`（Model Selection と派遣手順）
- Modify: `.agents/skills/subagent-driven-development/implementer-prompt.md`
- Modify: `.agents/skills/subagent-driven-development/task-reviewer-prompt.md`
- Modify: `.agents/skills/subagent-driven-development/re-review-prompt.md`
- Modify: `.agents/skills/subagent-driven-development/references/final-review.md`
- Modify: `.agents/skills/subagent-driven-development/references/fix-loop.md`
- Modify: `.agents/skills/subagent-driven-development/references/handling-reports.md`
- Modify: `.agents/skills/executing-plans/SKILL.md`
- Modify: `.agents/skills/writing-plans/SKILL.md`
- Modify: `.agents/skills/requesting-code-review/SKILL.md`
- Modify: `.agents/skills/requesting-code-review/code-reviewer.md`
- Modify: `tests/model-selection/scenarios.md`、`docs/superpowers/validation/cost-aware-model-selection.md`

**Interfaces:** Task 2 の選択手順に `mechanical`／`integration-debugging`／`design-high-risk`／`task-review`／`final-review` と brief・diff のパスを渡す。Task 3 で適用する。既存のモデル欄を維持して適用に必要な値を渡し、子へ料金比較を再委譲しない。

- [ ] **Step 1: 4スキルを変更前に確認する。** SDD、executing-plans、writing-plans、requesting-code-review の各原文で、低リスク最終レビュー、高リスク小差分、失敗原因別の再選択、縮小した再レビューの control を記録する。A01–A18・B01–B03・C01–C02 と対応させる。
- [ ] **Step 2: スキル単位で編集し、代表ケースを確認してから次へ進む。** SDD の選択節と参照先を共通手順に置換し、template に役割・選択結果・brief の参照を渡す。`most expensive one` の断定、最終レビューの最高能力固定、中位モデルの下限、根拠のない2–3倍の係数を除去する。
- [ ] **Step 3: 同じ原則を残りの3スキルへ接続する。** 特に writing-plans の execution handoff にある「最終レビューは最も高性能／Native は常に最安・最速」という案内を条件付きの比較へ変更する。reviewer template の model 欄を追加し、上位の caller に料金比較の責務があると明記する。
- [ ] **Step 4: 修正ループを検証する。** 1–3回目の再開、再開不能時の新規派遣、4–5回目の新規担当、5回目の breaker を維持する。4–5回目は不足能力の根拠による再選択とし、同じモデルを維持した場合も理由を記録する。ツール障害ではモデルを上げず、context 不足では情報整理を比較する。
- [ ] **Step 5: 代表ケースと主要回帰を確認する。** AC01–AC04・AC08・AC16・AC20 の選択記録とループ記録を確認する。小さな差分だから高リスクの条件を落とす、節約のためレビューを省く、同じ失敗を新しい round 1 とする挙動を失敗とする。各スキルの確認済み結果を残す。
- [ ] **Step 6: 変更したファイルだけをコミットする。** Message: `refactor: apply cost-aware selection to implementation workflows`。

### Task 5: 設計確認・並列調査・診断・スキル評価の移行

**Files:**
- Modify: `.agents/skills/brainstorming/SKILL.md`
- Modify: `.agents/skills/brainstorming/builder-check-prompt.md`
- Modify: `.agents/skills/dispatching-parallel-agents/SKILL.md`
- Modify: `.agents/skills/diagnosing-superpowers/SKILL.md`
- Modify: `.agents/skills/writing-skills/SKILL.md`
- Modify: `.agents/skills/writing-skills/references/testing-skills-with-subagents.md`
- Modify: `.agents/skills/writing-skills/references/CLAUDE_MD_TESTING.md`
- Modify: `tests/model-selection/scenarios.md`、`docs/superpowers/validation/cost-aware-model-selection.md`

**Interfaces:** 設計確認は `design-high-risk` または実際のリスクに応じた review、調査・診断は `research-audit`、テスト担当は `skill-evaluation` を使う。スキル評価の対象モデルは、通常の作業効率の比較より優先して固定する。

- [ ] **Step 1: 各変更スキルの control を記録する。** builder check、3件の並列調査、診断の analyst／scrub／similar-session、同じ variant の評価途中に安価なモデルへ変えたくなるケースを用意する。C03–C12 に対応付ける。
- [ ] **Step 2: brainstorming と dispatching-parallel-agents を順に接続・確認する。** role と入力量・リスクから共通ルールを参照する。読み取り主体でも見落としの影響は評価する。既存の独立性・並列可否・no-subagents 契約を変えない。
- [ ] **Step 3: diagnosing-superpowers を接続・確認する。** 7観点、scrub と scrub-audit、候補ごとの類似分析の各派遣で選択を適用する。既存の intake、読み取り制約、秘匿化、証拠引用、エクスポート承認を変更しない。診断対象ログから見えない実費を生成しない。
- [ ] **Step 4: writing-skills と2参照資料を接続・確認する。** テスト対象モデル・effort・runtime を variant 間で固定し、モデル別に結果を分ける。評価の子へ今回の選択ルールを読み込ませるかどうかを variant として明示し、control に新ルールを混入させない。`skill-evaluation` のモデルを勝手に安価なモデルへ変えない。
- [ ] **Step 5: AC15・AC16 と各派遣の観測を確認する。** 料金表を全員へ配布しないこと、選択専用子を作らないこと、7観点など既存の必要な検証担当を勝手に省かないことを確認する。各スキルの確認が完了してから次のスキルへ進んだ証拠を残す。
- [ ] **Step 6: 対象ファイルだけをコミットする。** Message: `refactor: apply model selection to research and skill evaluation`。

### Task 6: 35項目の照合、実機受入、導入ガイド

**Files:**
- Modify: `README.md`（現状は空。導入・運用手順を追加）
- Modify: `docs/superpowers/validation/cost-aware-model-selection.md`
- Modify: `tests/model-selection/scenarios.md`（検証で判明したケースのみ）
- 先行タスクのファイルに欠陥があればその箇所だけ修正し、対応するケースを再確認する。

**Interfaces:** 全タスクの記録、監査の35 ID、仕様の20 AC を受け取り、各行に証拠と `pass | fail | unverified` を持つ受入表を提供する。未知の実行結果を pass へ変換しない。

- [ ] **Step 1: 移行の抜けを走査する。** 実行: `rg -n --hidden -i 'most capable|more capable|mid-tier|cheap-to-mid|one tier above|2-3|2–3|most expensive' .agents/skills`。一致を一つずつ確認し、説明・反例・参考資料は理由付きで除外する。実行される選択命令の残存は修正する。35項目全てに更新先または非適用理由を記録する（AC17）。
- [ ] **Step 2: 文書の構造とリンクを確認する。** 実行: `git diff --check`。さらに `python3` の一時コマンドで変更 Markdown のローカル相対リンク・profile キー・カタログ参照・必須記録項目を検査する。リンク先を未配置の user 設定にしない。設定例の placeholder と動作する worker を区別する。
- [ ] **Step 3: 20 AC の受入表を埋める。** 固定値は Decimal の結果、行動は具体的なサンプル、移行はソース差分を証拠にする。AC01–AC20 それぞれについて、該当 Task の記録が揃っていることを確認する。すでに確認済みのケースは新しい変更・失敗がなければ反復しない。
- [ ] **Step 4: 各対応環境の実機受入を揃える。** Task 3で確認した記録は設定や文書の変更で無効になっていなければ再利用する。未確認・無効になった VS Code Local、VS Code Copilot、CLI について、利用可能な2候補で読み取り専用の同じ短い依頼を派遣し、要求モデル・実効値・バージョン・設定を確認する（AC18）。拒否、Auto／上書き、context 不明のケースも観測可能な範囲で確認する。環境未提供なら具体的な手順を残して unverified とし、独立して可能な文書確認は完了させる。実機確認のない方式を対応済みとは記載しない。
- [ ] **Step 5: README に導入・運用手順を書く。** 共通スキルの参照、環境の確認、必要な worker の配置と認識確認、選択記録例、料金更新、未検証時の表示、旧課金方式が対象外であることを説明する。ユーザー共通設定・MCP の変更を必須手順にしない。
- [ ] **Step 6: 作業範囲に応じた新しい担当の最終レビューを行う。** 選択に必要な能力と料金を確認してモデルを明示し、最高能力固定を使わない。計画・仕様・Review Focus・受入記録・全変更差分を渡す。指摘の修正後は該当ケースだけを再確認する。
- [ ] **Step 7: 対象ファイルだけをコミットする。** Message: `docs: record model selection validation and setup`。最終報告で、文書実装、行動検証、環境ごとの実機確認を区別し、未完了の受入条件を具体的に示す。

## 仕様の対応表

| 仕様の範囲 | 実装・確認するタスク |
| --- | --- |
| §1–4 目的・範囲・構成 | Task 1–3、Task 6 README |
| §5 モデル・料金・根拠・更新 | Task 1、Task 2の鮮度・欠落処理 |
| §6–8 選択・費用・コンテキスト・推論 | Task 2、適用はTask 3 |
| §9 実行環境差 | Task 3、実機受入はTask 6 |
| §10 再選択・例外・予算 | Task 2・4 |
| §11 記録・評価モデル固定 | Task 2・5 |
| §12 35項目の移行 | Task 3–5、最終照合はTask 6 |
| §13 20受入条件 | `scenarios.md` のAC索引とTask 6の証拠付き表 |
| §14 初期値と実装裁量 | 共通インターフェース、Global Constraints、Task 1–3 |
| §15 公式仕様の再確認 | Task 1のカタログとTask 3の環境ガイド |

## 実行順序と引き継ぎ

Task 1 → 2 → 3 → 4 → 5 → 6。環境の実機確認待ちは、その環境の確認だけを保留し、文書と独立した残りのタスクを進める。実機受入を省略して全体完了とはしない。

この計画では共通ルール・カタログ・検証記録を繰り返し参照し、既存スキルを順に移行するため **Native（担当がこのセッションで順に実装）を推奨**する。最後に独立した全体レビューを行う。タスクごとの新しい実装担当・レビュー担当を使う方式も選べるが、同じ資料の再読と引き継ぎが増える。

今回は計画作成のみ。実装開始時に実行方式を確認し、選択した方式のスキルと `writing-skills` に従う。
