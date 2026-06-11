# STATUS — 趣味情報パトロール隊（前提・判断の記録）

最終更新：2026-06-11
担当：code（Claude Code）

## 1. 何を前提に着工したか
- Claude Code Routines 公式ドキュメント（https://code.claude.com/docs/en/routines）を着工前に確認。リサーチプレビューのため推測で組まず仕様に沿って構成した。
- Notion指示書「🛰️ 趣味情報パトロール隊指示書」(page_id: 37c90fd5-9e77-8103-8a08-cc6038189c82) の巡回リスト8項目・報告ルールを転写してプロンプト化した。

## 2. 検証結果（指示書 #6：web検索の可否を最初に検証）
| 項目 | 結果 | 根拠 |
|------|------|------|
| WebSearch | **利用可** | 同一のAnthropicクラウド基盤上のセッションで実動作確認。Anthropic経由のためネット許可リスト非依存。 |
| WebFetch（個別ページ精読） | **Full なら可 / Default(Trusted) は不可** | Trustedは許可ドメイン外を `403 host_not_allowed` でブロック。→ 環境を **Full** に設定して回避（ユーザー承認済み）。 |
| Notionコネクタ | **利用可** | Routineにデフォルト付与・Anthropic経由でネット制限非依存。→ 出力先は指示書の子ページに確定。 |

## 3. 判明した制約と判断（回避策を発明せず確認済み）
- **Routine本体の作成はUI専用**（web/desktop/`/schedule`）。エージェントからは作成不可 → 人間がUIで作成する（SETUP.md 参照）。
- **`patrol` リポジトリをエージェントが作成できない**：`create_repository` が `403 Resource not accessible by integration`。GitHubスコープが `aliced0247/farm_game` 限定。
  - 指示書「farm_gameに相乗りしない」を尊重し、farm_gameへの書き込みは行わなかった。
  - ユーザー判断：**patrolを手動作成（推奨）** を選択 → 人間が `patrol` を作成し、本ファイル群を配置する。

## 4. 出力先と読み取り先
- **主系（primary）**：指示書ページ(37c90fd5-9e77-8103-8a08-cc6038189c82) の **子ページ**「📡 巡回報告 YYYY-MM-DD」。Notionコネクタが使えるためルールどおりNotionを採用。
- **フォールバック**：Notion書き込み失敗時のみ `patrol/REPORT.md` を上書き更新。
- **ブランチ制約（指示書 #5）**：Routineは既定で `claude/` 接頭辞ブランチにしかpushできない。
  - くろくんが `main` で読めるようにするには、patrolで **"Allow unrestricted branch pushes" を有効化**し、REPORT.md は **`main`** に書く（= 固定ブランチ = `main`）。
  - 有効化しない場合の固定ブランチは **`claude/patrol-daily`**（この名前を読みに行くこと）。
- **くろくんの読み取り先**：毎朝 = 指示書の最新の子ページ。落ちた日のみ上記フォールバックを参照。

## 5. 残作業（人間が実施）
1. GitHubで `patrol` を作成し本ファイル群を配置・コミット。
2. Claude GitHub App を `patrol` にインストール。
3. claude.ai/code/routines で Routine 作成（SETUP.md の手順2）。ネットワーク=Full、Daily 10:00 JST、Notionコネクタ残す。
4. Run now で動作確認（子ページ生成・⚠️期限セクションの有無）。

## 6. 未確定・要観察
- Routineが「リポジトリ0個」を許すか未確認のため、最低1リポジトリ（patrol）を指定する前提で設計。
- 試運転期間は巡回の質を見て指示書・プロンプトを調整（Routinesは仕様変動あり）。
