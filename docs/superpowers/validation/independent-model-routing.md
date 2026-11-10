# 独立モデルルーティング検証記録

日付: 2026-11-11。開始コミット: `1a7d903`。対象: [仕様](../specs/2026-11-11-independent-model-routing-design.md)／[計画](../plans/2026-11-11-independent-model-routing.md)。

## 実行条件

Native方式。元のmainを保持し、隔離worktreeのfeat/independent-model-routingで実装。既存unittest discoverは0件で、既存スイートの成功を意味しない。code/copilotコマンドなし。実機・実費・節約率は未検証。

基準原本は公式v7.0.2。初期80ファイルのtree・blob・mode一致を確認。[移行台帳](../../audits/2026-11-11-independent-model-routing-migration.md)／[互換性](../../../compatibility/superpowers.md)。旧検証は新構成へ転記しない。

## 受入マトリクス

passは記載した証拠範囲だけ。実機欄が未検証ならネイティブ成功ではない。

| ID | 条件 | 文書・算術・シミュレーション | 実機 |
| --- | --- | --- | --- |
| IR01 | 原本と独自機能の分離 | unverified | unverified |
| IR02 | 移設の完全性 | unverified | unverified |
| IR03 | 読み込み | unverified | unverified |
| IR04 | 重複の解消 | unverified | unverified |
| IR05 | 無改変本体との最終レビュー | unverified | unverified |
| IR06 | 高リスクと再レビュー | unverified | unverified |
| IR07 | 算術・未知値・境界値 | unverified | unverified |
| IR08 | 明示モデル指定 | unverified | unverified |
| IR09 | 拒否・Auto・上書き・変換 | unverified | unverified |
| IR10 | コンテキストと推論 | unverified | unverified |
| IR11 | 指定経路なし | unverified | unverified |
| IR12 | 本体ワークフロー | unverified | unverified |
| IR13 | 評価・調査・診断 | unverified | unverified |
| IR14 | 反復と圧縮 | unverified | unverified |
| IR15 | 導入・更新・解除 | unverified | unverified |
| IR16 | 本体更新 | unverified | unverified |
| IR17 | 報告の正確さ | unverified | unverified |
| IR18 | 予算・ユーザー指定 | unverified | unverified |

## タスク証拠

Task 1: 公式v7.0.2のskills treeと初期取込が完全一致。26変更／6追加、走査35項目、所有86ファイルを列挙。利用先の原本一致・重複解消は実機未確認。

Task 2: 独自6ファイルを抽出。カタログとプロファイルのJSONが保存コミットと等値、F01～F05はDecimalで一致。管理用配置と一時.github/skills配置で参照リンクを確認。原本の選択ロジックは維持し、環境ガイドへの参照だけを追加した。

Task 3: 方針2026-11-11.1の短い入口、配置先binding、正式な記録フィールド、環境ガイドの読込契約を作成。公式資料を2026-11-11再確認。D01の実際の取り込みは実機未検証。モデル名の文章指定と正式指定、実効不明と指定経路なしを区別した。

## Task 4: 原本との判断比較

要求評価設定: skill-evaluation / gpt-6.1-sol / medium / fork_turns none（全変種共通）。現在のCodex契約の価格は取得できず、費用比較は不可。Copilotカタログを価格根拠にしない。実効ID・設定・容量・費用はunknown。評価だけを委譲し、実装は親が行った。

| sample | control S01 | control S02 | control S04 | changed S01 | changed S02 | changed S04 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | b | b | 文章指定で続行 | a | b | 当該派遣保留・入力整理 |
| 2 | b | b | 自己レビューへ代替 | a | b | 当該派遣保留・入力整理 |
| 3 | b | b | 文章指定で続行 | a | b | 当該派遣保留・入力整理 |
| 4 | b | b | 指定不能で保留 | a | b | 当該派遣保留・入力整理 |
| 5 | b | b | 文章指定で続行 | a | b | 当該派遣保留・入力整理 |

観測RED: 通常最終レビューで全controlがbを固定選択。例: control-1の「Both workflows and final-review reference require the most capable available final reviewer, even though a costs $0.110 here.」。S04のcontrol-1/3/5は制御不能をledgerに記録して文章依頼で続行し、control-2は独立レビューを自己レビューへ代替した。control-4の保留は既に正しい挙動として記録し、失敗としない。

GREEN: changedの5sampleが通常レビューa（基本見積0.11USD）、高リスクb（0.204USD）を選び、S04は正式経路の発見まで当該派遣を保留。S03は不一致の再評価と、指定可能だが実効観測不能なケースの継続／unknownを区別。S05～S09も各5sampleで評価固定・再レビュー・役割維持・原因別対処・期限超過・予算保留・未知値除外・明示指定維持を確認した。単価だけでlookupを常に最安としない回答は正しい条件付判断とした。

5sampleの応答を親が読み、対象ケースに関する追加の方針修正は不要と判断した。[評価出力と入力ハッシュ](independent-model-routing-samples.md)を保存。少数のバッチ入力による質的回帰証拠であり、全作業・全モデル・全環境での遵守率や実際の節約額は示さない。正式なスキル発見・会話への取り込み・Copilot指定成功は別途未検証。

Task 5: 開始時から無変更の所有86ファイルだけをgit rmで除去。原本のtree一致、独自7資料の存在、抽出データと算術、判断試験を確認して実施。git ls-files .agents/skillsは0件。元mainとGit保存コミットは維持。D04の全利用先スコープは実機未確認。

Task 6: 個人／プロジェクト配置、LocalプロファイルとCopilot／CLIの違い、所有記録・更新・解除・旧方式復元を手順化。D02/D03を一時ファイルで再現し、空・日本語CRLF・UTF-8 BOM・末尾改行なし・空白パスで、既存部分保持、同版再導入の不変、所有外追記を保った更新・解除、管理部／SKILL編集／未所有ファイルのドリフト検出を確認。これは手順のバイト操作をPythonで再現した証拠であり、製品インストーラーやGUI実機試験ではない。実際のユーザー設定・MCP・親モデルは変更していない。
