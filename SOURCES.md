# Sources

| リポジトリ | 固定コミット | ライセンス | 使い方 |
|---|---|---|---|
| [0x000x7f/claude-code-agent-workflow](https://github.com/0x000x7f/claude-code-agent-workflow) | `dcede8f549c13bcc7d2f2a41940403f5c27a7c19`（2026-10-16） | MIT（`LICENSES/claude-code-agent-workflow.LICENSE`） | 要約（docs/01）と改変テンプレート（templates/） |

## templates/ での主な変更点

- `model:` をモデル ID（`claude-opus-4-8` / `claude-sonnet-4-6`）から別名（`opus` / `sonnet`）へ
- 説明文の「global agent」表記を削除（プロジェクト単位で導入するため）
- reviewer: lean-ctx の注記を削除、unity-cli の読み取り専用コマンドを許可、constitution と specs/ を検収基準に追加
- rules/agent-workflow.md: メイン会話のモデル名を一般化、Spec Kit の `specs/` を新機能の計画の正とする記述と Unity 用の追加ルールを追加
- 作者の環境専用のルール（output-style、remote-change-log、web-freshness）と `scripts/doctor.ps1` は含めない
