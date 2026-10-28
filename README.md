# .github

このアカウントの全リポジトリに共通の、Issue テンプレートと PR テンプレート (GitHub の [default community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file))。

## テンプレート

| ファイル | 種類 | 使うとき |
|---|---|---|
| `1-task.yml` | 作業 | 機能の追加、改善、保守、ドキュメント、インフラ。AI エージェントにそのまま渡せる形で書く |
| `2-bug.yml` | バグ | 期待と違う動き。再現手順と、直ったことの確かめ方 |
| `3-epic.yml` | 親 Issue | 複数の Issue を束ねる。子はサブ Issue で紐づける |
| `4-spike.yml` | 調査 | 分からないことを、時間を区切って調べる |
| `5-decision.yml` | 判断の記録 | どちらにするか決め、選択肢と理由を残す |
| `pull_request_template.md` | PR | 関連 Issue、確かめたこと、確かめていないこと、Risk |

空の Issue は作れない設定にしている (`config.yml`)。リポジトリの管理者には、空の Issue も選べる。

## 考え方

- **Issue は AI エージェントへの指示としても読まれる。** 作業の Issue は Goal、触るもの、触らないもの、完了条件、確かめ方、Risk を書く。1 Issue で 1 PR、触るファイルは 10 個以内を目安にする
- **後で変わる値はフォームで聞かない。** Status、優先度、見積りは GitHub Projects のフィールドで、期限はマイルストーンで、親子と依存は Issue の Relationships (サブ Issue、blocked by) で持つ。同じ情報を 2 箇所に書かない
- **種類はラベルで表す。** 個人アカウントでは Issue types が使えないため

## 注意

- **リポジトリに自前の `.github/ISSUE_TEMPLATE/` が 1 ファイルでもあると、ここの Issue テンプレートは丸ごと使われない。** PR テンプレートはファイルごとに判定される
- **private リポジトリでは、フォームの必須チェック (`required`) が効かない。** そのため、必須の欄はラベルに「(必須)」と書いている
- **テンプレートが付けるラベル (`bug`、`epic`、`spike`、`decision`) は、使う側のリポジトリに無いと付かない。** `bug` は GitHub の既定のラベルにある。ほかは必要なリポジトリに作る
- このリポジトリは public。テンプレートに、特定のプロジェクトの名前、ドメイン、認証情報を書かない

## 参考にしたもの

- GitHub Docs: [Syntax for issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)、[Best practices for Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/best-practices-for-projects)、[Get the best results from Copilot coding agent](https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results)
- [What Makes a GitHub Issue Ready for Copilot? (arXiv 2512.21426)](https://arxiv.org/abs/2512.21426): 範囲が絞られ、自己完結し、触るファイルが書かれた Issue ほど、エージェントの PR がマージされやすい
- 実例: vercel/next.js、hashicorp/terraform、anthropics/claude-code、astral-sh/uv (bug)、conda/conda、gitpod-io/gitpod、rust-lang/rust (epic、tracking issue)、FerretDB/FerretDB (chore の完了の定義)、kyma-project/lifecycle-manager (spike のタイムボックス)、adr/madr (decision)、antfu/.github (個人の共通テンプレート)
