# .github

このアカウントの全リポジトリに共通の、Issue テンプレートと PR テンプレート (GitHub の [default community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file))。

**使うのは AI エージェント。** Issue と PR は、Claude Code などのエージェントが `gh` で作る。そのため、ブラウザ向けの Issue フォーム (YAML) ではなく、`gh` と API から読める Markdown のテンプレートにしている。gh は Issue フォームを扱えない ([cli/cli#5865](https://github.com/cli/cli/issues/5865))。

## テンプレート

| ファイル | 種類 | 使うとき |
|---|---|---|
| `1-task.md` | 作業 | 機能の追加、改善、保守、ドキュメント、インフラ |
| `2-bug.md` | バグ | 期待と違う動き。再現手順と、直ったことの確かめ方 |
| `3-epic.md` | 親 Issue | 複数の Issue を束ねる。子はサブ Issue で紐づける |
| `4-spike.md` | 調査 | 分からないことを、時間を区切って調べる |
| `5-decision.md` | 判断の記録 | どちらにするか決め、選択肢と理由を残す |
| `pull_request_template.md` | PR | 関連 Issue、確かめたこと、確かめていないこと、Risk |

## エージェントの使い方

テンプレートの本文を読み、埋めて、`--body-file` で渡す。

```sh
# テンプレートの一覧と本文 (既定のテンプレートも含めて、そのリポジトリで使えるもの)
gh api graphql -f query='query($o:String!,$n:String!){repository(owner:$o,name:$n){issueTemplates{name labels{nodes{name}} body}}}' -F o=<owner> -F n=<repo>

# 立てる
gh issue create -R <owner>/<repo> --title "..." --body-file issue.md --label <ラベル>
```

- テンプレートの `<!-- -->` のコメントは書き方の説明。埋めたら消す
- 親 Issue は `--parent`、依存は `--blocked-by` で付ける。本文に書かない
- Status、優先度、見積り、期限は書かない。Projects のフィールドとマイルストーンで持つ

## 考え方

- **Issue は実装するエージェントへの指示として読まれる。** 作業の Issue は Goal、触るもの、触らないもの、完了条件、確かめ方、Risk を書く。1 Issue で 1 PR、触るファイルは 10 個以内を目安にする
- **後で変わる値は本文に書かない。** 同じ情報を 2 箇所に持たない (GitHub の [Best practices for Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/best-practices-for-projects) の single source of truth)
- **種類はラベルで表す。** 個人アカウントでは Issue types が使えないため

## 注意

- **リポジトリに自前の `.github/ISSUE_TEMPLATE/` が 1 ファイルでもあると、ここの Issue テンプレートは丸ごと使われない。** PR テンプレートはファイルごとに判定される
- **テンプレートが示すラベル (`bug`、`epic`、`spike`、`decision`) は、使う側のリポジトリに無いと付かない。** `bug` は GitHub の既定のラベルにある
- このリポジトリは public。テンプレートに、特定のプロジェクトの名前、ドメイン、認証情報を書かない

## 参考にしたもの

- GitHub Docs: [Get the best results from Copilot coding agent](https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results)
- [What Makes a GitHub Issue Ready for Copilot? (arXiv 2512.21426)](https://arxiv.org/abs/2512.21426): 範囲が絞られ、自己完結し、触るファイルが書かれた Issue ほど、エージェントの PR がマージされやすい
- 実例: vercel/next.js、hashicorp/terraform、anthropics/claude-code (bug)、conda/conda、gitpod-io/gitpod、rust-lang/rust (epic、tracking issue)、FerretDB/FerretDB (chore の完了の定義)、kyma-project/lifecycle-manager (spike のタイムボックス)、adr/madr (decision)
