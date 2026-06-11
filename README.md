# patrol — 趣味情報パトロール隊

もえちゃんの趣味情報を Claude Code Routines が毎日10:00 JSTにクラウドで巡回収集し、日次報告を作るシステム。

## 中身
| ファイル | 役割 |
|----------|------|
| `ROUTINE_PROMPT.txt` | Routine の Instructions 欄に貼るプロンプト本体（指示書の巡回リスト8項目＋報告ルールを転写） |
| `SETUP.md` | リポジトリ・Routine の作成手順（人間が一度だけ実施） |
| `STATUS.md` | 前提・検証結果・判断・残作業の記録 |
| `REPORT.md` | Notion 出力が落ちた日だけ使うフォールバック報告先 |

## 仕組み（概要）
- **トリガー**：Schedule / 毎日 10:00 JST
- **出力（主系）**：Notion 指示書ページの子ページ「📡 巡回報告 YYYY-MM-DD」
- **出力（フォールバック）**：`REPORT.md`
- **情報源**：WebSearch ＋（Full ネットワークで）WebFetch。X は対象外（Grok 分業）
- **鉄則**：期限あり情報は報告冒頭の「⚠️ 期限」セクションに集約。推測・捏造禁止。

詳細は `SETUP.md` と `STATUS.md` を参照。
