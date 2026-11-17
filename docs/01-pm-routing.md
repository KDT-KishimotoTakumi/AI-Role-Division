# 01. PM ルーティング（claude-code-agent-workflow）

出典: [0x000x7f/claude-code-agent-workflow](https://github.com/0x000x7f/claude-code-agent-workflow)（MIT、commit `dcede8f`）

## 考え方

メイン会話を PM / 設計役にし、作業の**複雑さと量**で担当を振り分ける。

### 3原則

1. **サブエージェントはコンテキスト汚染対策**。大量の読解・ログ・検索結果などメイン会話を汚す作業だけを切り出す。
   短い作業・小修正には使わない（起動コストと再説明の負担が上回る）。
2. **文脈はファイルで共有する**。調査結果・計画・指示は `docs/` などの Markdown に残す。他ツール・別セッション・人と共有できる。
3. **高価なモデルは価値の高い工程に使う**。実装を安いモデルに渡すのは能力不足だからではなく、PM・設計・検収に上位モデルを残すため。

## 担当

| 作業 | 担当 | モデル |
|---|---|---|
| 大量のファイル読解・調査 | Explore（組み込み） | — |
| 実装計画・設計 | Plan（組み込み）／プランモード | — |
| 単純・定型・大量の編集（lint、型エラー、リネーム） | implementer | Sonnet |
| 複数ファイルにまたがる中程度の実装 | implementer-heavy | Opus |
| 重い設計判断・アーキテクチャ変更 | メイン会話が続投 | セッションのモデル |
| usage limit が近い／長時間の定型実装 | `/handoff` で Codex / Cursor へ | 外部 |
| 差分の検収 | reviewer（読み取り専用） | セッションのモデル |

## コマンド

| コマンド | 内容 |
|---|---|
| `/analyze-codebase [対象]` | 編集せずに調査し、`docs/refactor-plan.md` に Phase 分けの計画を書く |
| `/implement-phase <N>` | 計画の Phase N を、担当を判断して実装し、reviewer で検収する |
| `/review-diff [範囲]` | reviewer で差分を検収する（他ツールの戻り差分も） |
| `/handoff <topic>` | このセッションを知らない実装者でも作業できる指示書を `docs/handoff/` に書く |

## 検収が必須の範囲

1. Codex / Cursor など他ツールから戻ってきた差分
2. `/implement-phase` の成果

それ以外の自前の小さな差分は任意（原則1を優先）。

## 報告の形式

- implementer: 変更ファイル／理由／挙動が変わっていない根拠／テスト結果／差し戻した判断
- implementer-heavy: 変更ファイル／設計判断と理由／既存仕様への影響／テスト結果／残課題
- reviewer: 判定（approve / request changes / comment）／重大な問題／軽微な問題／追加テスト案／次へ進めるか

## 制限（元リポジトリの記載）

- reviewer の Bash 制限はプロンプトでの約束であり、完全な権限分離ではない
- `commands/` は旧形式（将来 `skills/` へ移行候補）
