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
