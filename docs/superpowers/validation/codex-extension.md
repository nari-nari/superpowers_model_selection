# Codex拡張の検証記録

日付: 2026-11-11（Asia/Tokyo）。[仕様](../specs/2026-11-11-codex-extension-validation-design.md)・[計画](../plans/2026-11-11-codex-extension-validation.md)はユーザー承認済み。開始コミット `e29b793bd417b7413f01a5171d4e212d90d03279`。

## 実行環境と範囲

管理変更は隔離branch `feat/codex-extension-validation`。元mainと元の未追跡仕様・計画を保持。ChatGPTアカウントによるPlusログインはユーザー回答で確認。現在利用中の枠が含有枠か追加クレジットか、speedと残量、対象アカウントの実モデルIDは未確認。

この会話は拡張由来と確認できていない。GUI操作ツールなし。CLIで推論を実行して代理試験にはしない。インストールファイルではCodex拡張 `26.51007.21434` と `26.51021.22027` を観測したが、稼働版はunknown。公式本体はこの会話のスキル一覧でv7.0.1のソースパスを観測しただけで、拡張側の有効ソース・版・原本一致は未確認。

## 受入台帳

| ID | 文書・ローカル確認 | 拡張実機 | 理由／証拠 |
| --- | --- | --- | --- |
| CX01 | 環境・出所の記録を準備 | unverified | 稼働版・エンジン版・拡張本体ソースはunknown |
| CX02 | 読込手順を準備 | unverified | 拡張の新会話と読込履歴が未取得 |
| CX03 | 同一fixtureと2候補の手順を準備 | unverified | 実ID・正式引数・独立した実効IDが未取得 |
| CX04 | 拒否・fallback・未知設定の期待動作を定義 | unverified | 拡張で未再現 |
| CX05 | 既存S01・S02・S05・S08を再利用 | unverified | 実機の判断試験未実施 |
| CX06 | 契約別料金の取得先を準備 | unverified | Plus確認のみ。課金枠・候補利用可否は未確認 |
| CX07 | 呼出し前後の採取手順を準備 | unverified | 呼出しなし。usage／actual_costはunknown |
| CX08 | S06との比較手順を準備 | unverified | 拡張側本体ワークフロー未確認 |
| CX09 | 所有物の配置・解除確認を準備 | unverified | 実機解除後の新会話未確認 |
| CX10 | S07と復元手順を準備 | unverified | 拡張で再開・圧縮未再現 |

実機passには各ケースの独立した証拠が必要。ファイル版のみ、CLI成功、モデル自己申告の3反例はすべてunverified。資料のチェック結果だけで受入を昇格しない。

## 共通記録の初期値

```yaml
case_id: CX01
runtime: codex-vscode
surface_evidence: unavailable
extension_version: unknown
engine_version: unknown
superpowers_revision: unknown
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

Task 1～3: ケース・初期台帳、契約別参照、Codex用ガイドと記録形式、隔離試験配置、所有物の解除手順まで準備済み。Task 4は拡張の事前確認待ち。Task 5の状態表示は更新したが、実機解除は未実施。試験配置は再開用に保持している。

## 参照取得の変更後sample

同じ問いと評価設定で、Codexアダプター、条件付き料金表、固定モデル試験と費用保留の区別、Codex記録形式を取得できた。評価者は「入口の本文・所有記録・単独手順書がスキル資料だけには含まれない」点を残るギャップとして挙げた。この指摘を受け、同じ原本配布から入口と手順書を取得すること、試験配置ではAGENTS.md・model-routing-setup.md・.routing-local/ownership.jsonへ辿ることをガイドに明記し、実際に別配布した。変更後sampleを完全合格や実機遵守の証拠としては扱わない。

## ローカル確認の証拠

- 開始コミットと現版のCopilotカタログ／プロファイルのJSONを `json.loads` で比較: 等値。
- 既存F01～F05をDecimalで再計算: 全5件一致。
- overlay・導入手順・新仕様／計画／ケース／検証記録・README・互換性の16資料: 相対リンクと空白を確認。
- CX01～CX10が仕様・ケース・台帳で網羅されること、実機行がunverifiedのままであることを確認。
- 試験配置: 全8スキルファイルのバイト一致、相対リンク、入口binding、単独手順書、fixtureを確認。案内等を含む所有配布物14件のSHA-256をローカル所有記録へ保存。
- 使い捨てコピーで全14配布物の同版更新、1所有ファイルのドリフトで操作保留、未所有の日本語CRLFファイル保持、所有物だけの解除、ローカル結果記録の保持を再現。
- `git diff --check`: 問題なし。確認用PythonはGit対象外の当該計画workspaceで使い捨て実行し、製品コード／インストーラー／常設テスト基盤は追加していない。

上記は資料・バイト操作の確認であり、拡張の読込、モデル実適用、使用量や節約額を確認した結果ではない。

## 再開に必要な操作

準備済み試験フォルダのSTART-HERE.mdを開き、VS CodeのCodex拡張の新規会話へPREFLIGHT-PROMPT.txtを貼る。まず子を起動しない。結果は試験フォルダの `.routing-local/preflight.md` に保存する依頼で、保存不能なら非機密な会話結果だけでもよい。

この会話で終了を知らせてもらえばローカル結果を読み、読込と正式指定の前提を照合できる。対象拡張の有効ソース、稼働版、実ID2候補、容量と対応設定、usage観測が未確認なら該当試験は保留する。個人設定の変更、CLIの代理推論、観測だけの再呼出しはしない。試験終了後に所有物だけ解除し、別の新しい拡張会話で非読込を確認する。
