# 趣味情報パトロール隊 — セットアップ手順

Claude Code Routines はプログラムから作成できず、**web UI / デスクトップアプリ / CLI `/schedule`** でしか作れません（リサーチプレビュー）。
以下は人間（もえちゃん / くろくん）が一度だけ実施する手順です。完了すれば、以後は毎朝10:00 JSTに自動で巡回報告が作られます。

## 0. 前提
- Claude Code on the web が使えるプラン（Pro / Max / Team / Enterprise）
- Notionコネクタが claude.ai に接続済み（指示書ページにアクセスできること）

## 1. リポジトリ `patrol` を作成
1. GitHub で新規リポジトリ `patrol` を作成（private 推奨）。
2. このフォルダの `ROUTINE_PROMPT.txt` / `REPORT.md` / `STATUS.md` / `README.md` を `patrol` に置いてコミット。
3. **Claude GitHub App を `patrol` にインストール**（claude.ai の連携設定から、または Routine 作成フォームの指示に従う）。

## 2. Routine を作成（claude.ai/code/routines → New routine）
1. **名前**：`趣味情報パトロール隊`
2. **プロンプト**：`ROUTINE_PROMPT.txt` の中身を丸ごと Instructions 欄に貼り付け。モデルは Claude の最新（Opus 4.8 など）を選択。
3. **リポジトリ**：`patrol` を選択。
4. **環境（Environment）**：
   - ネットワークアクセスを **Full** に設定（ニュース/公式/チケットページを WebFetch で精読するため）。
   - ※ Default(Trusted) のままだと特定ページの精読が `403 host_not_allowed` でブロックされる。
5. **トリガー**：**Schedule → Daily**、時刻は **10:00**（ローカルタイムゾーンが JST であることを確認。数分の stagger あり）。
6. **Connectors**：**Notion を必ず残す**（出力先がNotion子ページのため）。不要なコネクタは外す。
7. **Permissions**：（任意）`patrol` で **Allow unrestricted branch pushes** を有効化。
   - REPORT.md フォールバックを `main` に書き込ませたい場合に必要。Notion出力が主系なので必須ではない。
8. **Create** をクリック。

## 3. 動作確認
- Routine 詳細ページの **Run now** で即時実行し、セッションを開いて以下を確認：
  - WebSearch が動いているか
  - 指示書ページの子ページに「📡 巡回報告 YYYY-MM-DD」が作られたか
  - 報告の冒頭に **⚠️ 期限** セクションがあるか
- ※ 実行リストの「緑」はセッションが正常終了した印で、タスク成功を意味しない。必ず中身を確認すること。

## 4. 運用
- くろくんは毎朝、**指示書ページの最新の子ページ**（= 当日の巡回報告）を読み、⚠️期限を拾って伝える。
- Notion出力が落ちた日のみ `patrol/REPORT.md`（フォールバック）を見る。詳細は STATUS.md 参照。
- 巡回内容を変えたいときは指示書の「巡回リスト」を編集し、`ROUTINE_PROMPT.txt` と Routine の Instructions を同期更新する。
