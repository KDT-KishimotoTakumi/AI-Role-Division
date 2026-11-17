# 02. Unity での実例（BallCollect）

Unity 6（6000.6.4f1）で、5個のアイテムを集める小さなゲーム「BallCollect」を作るときの役割分担。
エンジン操作は unity-cli（CLI → Unity 側の Bridge → Editor）、仕様管理は Spec Kit を使う。

## 使う道具

| 層 | 道具 | 役割 |
|---|---|---|
| 原則 | `.specify/memory/constitution.md` | テスト先行、unity-cli で再現、Play で確認、シンプルさ |
| 仕様・計画 | Spec Kit（`/speckit-specify` → `/speckit-plan` → `/speckit-tasks` → `/speckit-implement`） | spec.md / plan.md / tasks.md / quickstart.md |
| 担当の振り分け | `.claude/agents`・`commands`・`rules`（本リポジトリの `templates/`） | PM・implementer・reviewer |
| エンジン操作 | unity-cli スキル（scene-create、csharp-edit、playmode-testing など） | シーン・C#・入力・テスト・Play 検証 |
| 他ツール | `AGENTS.md` | Codex / Cursor が読む前提とテストコマンド |

## 工程と担当

| 工程 | 担当 | 具体的な作業 |
|---|---|---|
| 1. 原則 | メイン会話 | `/speckit-constitution` |
| 2. 仕様 | メイン会話＋ユーザー | `/speckit-specify`：B1〜B6 の条件、範囲外（敵・効果音など）を明記 |
| 3. 計画 | メイン会話 | `/speckit-plan`：純粋 C# の `CollectionState` を分離、Input System のアセット、確認用の名前の約束 |
| 4. 作業分割 | メイン会話 | `/speckit-tasks`：シーン → 移動 → ルールの3段階 |
| 5. テスト先行 | implementer-heavy でも可（Editor に触れない） | EditMode テスト（重複回収、5個でクリア、初期化） |
| 6. シーン構築 | メイン会話（Editor 操作） | unity-cli でオブジェクト作成・参照設定・保存 |
| 7. スクリプト実装 | implementer-heavy（複数ファイル） | `GameManager` / `PlayerController` / `CollectibleItem` / `HudView` |
| 8. 検収 | reviewer | 差分・原則・コンパイル状態・テスト結果の確認 |
| 9. Play 検証 | メイン会話（Editor 操作） | 入力のシミュレーション → 状態を取得 → 画面を撮影、verification.md に記録 |
| 10. 追加ルール | ユーザーが選ぶ → 別機能として 2〜9 を繰り返す | 例：30秒の時間制限 |

## 守ること

- 「C# を書いた」は完了ではない。コンパイルエラー0、テスト成功、Play で受け入れ条件を確認して完了。
- 座標や回収数を CLI から直接書き換えて「動いた」としない。入力して、状態を読み直して比べる。
- 確認済み・未成立・未確認を分けて記録する。短縮して試したら、そう書く。
- 1つの Editor を同時に複数の担当で操作しない。
