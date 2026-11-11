# Codex拡張での独立モデルルーティング検証 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** このPCのCodex拡張で独立レイヤーの読込・モデル指定・契約に応じた消費量比較を検証し、確認範囲と未検証条件を保存する。

**Architecture:** 公式本体を無改変のまま、共通選択スキルへCodex用の契約・環境ガイドを追加する。新規の隔離プロジェクトで試験し、拡張由来の証拠だけで実機受入を判定する。料金不明でも固定モデルの指定経路試験は進め、費用選択とは区別する。

**Tech Stack:** Markdown、公式Codex拡張とネイティブ子エージェント、Git、Python標準ライブラリによる資料整合性・算術確認。実行時コードと依存追加なし。

**Spec:** [Codex拡張での検証仕様](../specs/2026-11-11-codex-extension-validation-design.md)

**Status:** 2026-11-11にユーザーが承認。文書実装と、上限付き実機試験・判断シミュレーション・解除／新会話での非取込確認を実施して記録済み。拒否の条件未充足、provider実効・実費・実際の修正／圧縮などは未検証として終了。全面的な実機受入完了とは扱わない。

## Global Constraints

- 公式Superpowers本体・プラグインキャッシュを変更しない。
- 個人のCodex設定、既存スキル、Copilot設定、MCP、親モデルの既定値は変更しない。
- Copilotカタログの料金・利用可否・モデルIDをCodexへ流用しない。既存のCopilot候補JSONと算術F01～F05を保持する。
- 観測できない値は `unknown`、未実施・証拠不足の受入は `unverified`。
- 試験は同時1子、各依頼は小さな読み取り中心とし、自動反復しない。
- 料金鮮度は30日、コンテキストの余裕は20%、代表候補は最大3件を維持する。
- 初回は各モデル1回の読み取り試験まで。CX04は安全な拒否を最大1回だけ再現する。
- runtimeは `codex-vscode`、方針改訂時は `2026-11-11.2`、Codexカタログ初版は `2026-11-11.codex.1`。
- ChatGPTログインは確認済み。ユーザーがPlusと確認したため、現在の契約値は `chatgpt-plus-bucket-unknown`。適用枠・価格は未確認。
- GUI操作がこのセッションからできなければ、操作依頼と必要な最小証拠を一括案内する。CLIやこのチャットの子実行で拡張実機試験を代替しない。

## Review Focus

| 条件 | 期待する動作 | 担当 |
| --- | --- | --- |
| ChatGPTログインだが料金・消費量の帰属が不明 | 費用選択を保留し、固定モデルの能力試験と区別 | Task 2・4、CX06～07 |
| インストール済み版と実行中の版が異なる | 拡張の実セッションに版と証拠を結び付ける | Task 1・4、CX01 |
| CLIまたは別harnessの成功が混在 | 拡張由来の証拠だけを実機判定へ使う | Task 4、CX02～04 |
| モデル指定受理後の実効不明・fallback | 要求値と実効値を分離し、未検証を残す | Task 2・4、CX03～04 |
| 配布ファイル増加と既存6ファイル固定手順 | 実一覧の所有記録で更新・解除し、未所有物を保持 | Task 3・5、CX09 |

---

### Task 1: 環境記録と試験ケースを準備する

**Files:** Create: `docs/superpowers/validation/codex-extension.md`、`tests/model-selection/codex-extension-scenarios.md`。

**Interfaces:** Consumes: 仕様CX01～CX10、開始HEAD、拡張ファイルの既知情報。Produces: 全ケース未検証の受入台帳、出所を伴う環境記録、後続が使う同一fixtureと依頼文。

- [x] **Step 1:** `using-git-worktrees` に沿って隔離を確認し、`git status --short` と `git rev-parse HEAD` を記録する。ユーザー差分を移動・削除しない。
- [x] **Step 2:** 検証記録に仕様§8のフィールドとCX01～CX10の初期行を作る。実行中版と契約詳細をunknownで開始し、観測済みのファイル版とは別欄にする。
- [x] **Step 3:** ケース資料に各IDの入力、期待動作、必要証拠、再現不能時の状態を記載する。CX03は同じ `fixture.txt` の1行 `routing-fixture: codex-extension-v1` を返す依頼とし、モデル以外の入力を揃える。CX05は既存S01・S02・S05・S08、CX08はS06、CX10はS07を参照し、fixtureであることを明示する。
- [x] **Step 4:** 仕様のCX ID集合と両資料のケース集合を照合する。ファイル版のみ、CLI由来、自己申告しかない3例を当てはめ、実機passにならないことを確認する。`git diff --check` を実行する。

### Task 2: 契約別の選択・記録とCodexガイドを接続する

**Files:** Create: `overlay/skills/model-selection/references/codex-model-catalog.md`、`overlay/skills/model-selection/references/runtimes/codex-vscode.md`。Modify: 同スキルの `SKILL.md`、`references/task-profiles.md`、`references/decision-record.md`、`overlay/instructions/model-routing.md`。

**Interfaces:** Consumes: Task 1のケースと契約未確定状態。Produces: runtime／契約から正しい資料へ進む指示、Codexカタログ、正式指定と観測の手順。料金不明時は費用選択不可という状態を後続へ渡す。

- [x] **Step 1:** 公式OpenAI文書を取得し、スキル認識先、当該版の子指定とcustom agentの条件、ChatGPT契約の料金定義を確認する。プランや実IDが未確認の項目はunknownのまま出典・確認日を付ける。認証ファイル全体を開かない。
- [x] **Step 2:** 共通スキルの最初の参照を契約別に分岐させ、Codexガイドとカタログを追加する。Copilot候補JSONは保持し、Codexでは作業能力要件だけを再利用して当該候補の適格性を別確認する。
- [x] **Step 3:** 費用記録を仕様§8の単位付き形式へ接続し、既存 `estimate_credits` はCopilot用として保持する。Copilot換算の適用条件と、Codexの未知価格・未知実効値・固定モデル能力試験の区別を明記する。共通方針版を仕様の値へ更新する。
- [x] **Step 4:** CX05・06の既知入力で、Copilot換算混入、未価格最安判定、通常レビューの最高価格固定が起きないかを資料横断で確認する。既存S01～S09の期待動作を変更しない。これは人による契約確認で、LLM遵守や実機成功とは記録しない。
- [x] **Step 5:** 開始コミットと現在のCopilotカタログ／プロファイル内の既存JSONを `json.loads` で等値比較する。既存F01～F05を既存計画Task 2 Step 3のDecimal計算で再確認し、全5件一致を記録する。管理用配置の相対Markdownリンクと `git diff --check` を確認する。

### Task 3: 隔離試験の配置・更新・解除を準備する

**Files:** Modify: `docs/model-routing-setup.md`、`overlay/skills/model-selection/references/runtimes/codex-vscode.md`、同ディレクトリの `vscode.md`・`copilot-cli.md` の配布集合に関する記述。Create at trial location: `AGENTS.md`、`.agents/skills/model-selection/`、`model-routing-setup.md`、`fixture.txt`。モデル固定workerは必要時だけ `.codex/agents/` に作る。

**Interfaces:** Consumes: Task 2の配布資料と版。Produces: 新規試験ディレクトリ、ローカル所有ファイル一覧／ハッシュ／binding、単独で使える操作・解除手順。共有記録には個人絶対パスを載せない。

- [x] **Step 1:** 既存導入手順の「6ファイル」固定箇所と関連README・ガイドを検索し、実配布集合の記録へ整合させる。Copilot方式の既存部分保存・更新・ドリフト検出の契約を維持する。
- [x] **Step 2:** `/private/tmp` 配下の新規ディレクトリを選び、仕様§6の資料だけを配置する。共通入口の実本文と実在するbindingを使い、SHA-256と全リンクを照合する。公式本体の有効ソースと同名重複を読み取り確認し、未知の既存コピーは削除しない。
- [x] **Step 3:** 一時コピーで配置・同版更新・1所有ファイルの変更検出・1未所有ファイルの保存・解除を試す。所有一覧が実配布集合と一致し、ドリフト時の破壊的操作が止まることを確認する。Python標準ライブラリの使い捨て確認でよく、製品インストーラーや新規テスト基盤を作らない。
- [x] **Step 4:** ユーザーが試験フォルダをVS Codeで開き、新規Codex会話でCX01・02を確認できる短い操作案内を作る。求める情報は稼働版・契約種別・読込履歴・ツールの公開範囲に絞る。ここまでを準備済みと記録する。

### Task 4: Codex拡張で受入を実行する

**Files:** Modify: `docs/superpowers/validation/codex-extension.md`。必要時: ローカル試験プロジェクトのモデル固定workerと所有記録。

**Interfaces:** Consumes: Task 3の試験フォルダ、拡張の実セッション、確認した候補IDと権限。Produces: CX01～CX10の実施結果と証拠、未確認項目の再開条件。モデル実行はこのタスクだけで行う。

- [x] **Step 1:** 拡張セッションの識別・稼働版・本体ソースと版・契約を確認しCX01を更新する。現在チャットの出所が確認できなければ、Task 3の案内を一括提示し、拡張由来の結果を待つ。CLIで代用しない。
- [x] **Step 2:** 拡張の新会話で入口、binding、SKILLの読込と版を確認する。CX02が未確認なら依存するモデル試験を開始せず、必要な読込設定を特定する。未知の全体設定は書き換えない。
- [x] **Step 3:** 実ID2候補と必要能力・権限・容量・推論対応を確認する。正式引数がなければ、対応が確認できたプロジェクト内workerだけを使う。経路・容量が確定しなければ該当試験はunverifiedにする。CX07用の使用量観測経路もこの時点で確認し、取れない値はunknownとして続行する。
- [x] **Step 4:** CX03の同一fixtureを各モデルで1回ずつ、同時1子で読む。固定モデル能力試験か費用選択済み試験かを明記し、呼出し引数、子スレッド、独立した実効値を記録する。差分方式の使用量は各呼出し前後に取得し、後日復元できると仮定しない。自己申告・応答の正しさだけでpassにしない。
- [x] **Step 5:** 安全な指定拒否を最大1回再現する。実施記録: 局所拒否の根拠が得られず条件付き拒否は0回/unverified。判断ケースと役割・上限の比較は拡張の親で実施済み。実際に確認できたfallback・未対応設定・上書きだけをCX04へ記録し、未再現枝はunverifiedにする。CX05・08・10の判断ケースは読み取り中心の会話で確認し、ネイティブ／判断のみを区別する。
- [x] **Step 6:** CX06は当該契約の料金根拠が揃った場合だけ見積を比較する。CX07はStep 4の使用量を再利用し、観測目的だけの追加呼出しを行わない。集約残量しかなければ帰属不能として実費unknownを維持する。

### Task 5: 解除・互換性表示・残項目を確定する

**Files:** Modify: `docs/superpowers/validation/codex-extension.md`、`compatibility/superpowers.md`、`README.md`。Read: ローカル所有記録。

**Interfaces:** Consumes: Task 4の証拠とTask 3の所有記録。Produces: 確認範囲を限定した公開状態、再開手順、試験配置解除の証拠。

- [x] **Step 1:** 所有ハッシュを照合し、試験配置の所有物だけを解除する。実施記録: 所有13ファイルを解除し結果保持用.gitignoreを残した。新しい拡張会話で非取込を独立確認済み。変更済み・未所有物は保持して該当解除を保留する。解除後の新会話で入口が取り込まれないことを確認できた場合だけCX09をpassにする。
- [x] **Step 2:** READMEと互換性へCodexの別行と4段階の確認範囲を追加する。既存3環境は未検証のまま保持する。各passが拡張の証拠へ辿れ、unknownを成功へ丸めていないか照合する。
- [x] **Step 3:** `verification-before-completion` に従い、全変更の `git diff --check`、管理用／試験配置時のリンク結果、Copilot JSON等値と算術結果を確認する。実機結果はCX台帳の証拠から判定する。0件のunittestや資料検査を実機検証成功としない。
- [x] **Step 4:** 必須レビューを実行時のモデル選択方針に従って行い、未解決の不備を修正する。料金・実効値の不足を最高価格モデル固定や無根拠な追加派遣で補わない。変更した文書と検証範囲をユーザーへ提示し、自動push・常設導入は行わない。

## 実行前レビュー

本計画と仕様は2026-11-11に承認済み。Task 1～3の文書実装と試験配置を実施。Task 4は拡張の入口・スキル読込を確認し、同じwindowの候補UIと稼働版を取得。2モデル試験は経路・runtime設定・fixture・子別usageを確認済み。provider実効・実費はunknown。判断6ケースと有効本体の比較を記録し、Task 5の解除と新会話の非取込を確認して今回の作業を終了した。各checkboxは実施・処置の記録で、全受入の全面passを意味しない。
