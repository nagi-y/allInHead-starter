---
name: issue-run
description: "GitHub Issueを実装する。Issue番号を指定するか一覧から選んで、実装→コミット→PR作成まで自律的に進める。コードを書く仕事があるとき、バックログを消化したいときに使う"
user-invocable: true
disable-model-invocation: false
allowed-tools: Bash(gh *), Bash(git *), Read, Write, Edit, Glob, Grep
---

# Issue ドリブン実装ループ

GitHub Issue を起点に「読む → 実装 → PR 作成」を自律的に回す。

## 使い方

```
/issue-run              # オープンな Issue を一覧表示して選択
/issue-run 42           # Issue #42 を直接指定して実装
/issue-run --list       # 一覧だけ表示（実装しない）
```

---

## Phase 1: プリフライトチェック

### 1-1. git 作業ツリーの確認

```bash
git status --short
```

コミットされていない変更がある場合は、先にコミットを片付けてもらう。

### 1-2. gh 認証確認

```bash
gh auth status 2>&1 | head -3
```

---

## Phase 2: Issue 選択

### `$ARGUMENTS` が空 / `--list` の場合

```bash
gh issue list --state open --limit 30 \
  --json number,title,labels,updatedAt \
  | jq -r '.[] | "#\(.number)\t[\(.labels | map(.name) | join(", "))]\t\(.title)"'
```

一覧をユーザーに見せて「どの Issue を実装する？」と確認する。

---

## Phase 3: Issue 詳細の読み込み

```bash
gh issue view {number} --json number,title,body,labels,comments
```

**タスクタイプによって分岐:**

| タスクタイプ | 次の Phase |
|-------------|-----------|
| `implementation` | Phase 4（ブランチ作成）へ |
| `research` | Phase 3.5（調査実行）へ |
| `design` | Phase 3.5（設計まとめ）へ |

---

## Phase 3.5: 調査・設計系の実行（research / design のみ）

調査・設計を進め、結果を Issue コメントに残す。

```bash
gh issue comment {number} --body "調査結果 / 設計まとめ"
```

結論が出たら Issue を close。次のアクションが必要なら新しい Issue を提案。

**ここで終了。Phase 4 以降には進まない。**

---

## Phase 4: ブランチ作成

```bash
git checkout -b issue-{number}-{短い英語説明}
```

---

## Phase 5: 実装

- こまめにコミットする（論理的なまとまりで）
- コミットメッセージに `#Issue番号` を含める
- 実装中に判断が必要なことが出たらユーザーに確認する

---

## Phase 6: PR 作成

```bash
gh pr create \
  --title "{Issue タイトル}" \
  --body "Closes #{number}"
```

PR URL をユーザーに共有して完了報告する。

---

## 注意事項

- **1回の実行で 1 Issue** — まず確実に1つ完成させる
- **Phase 3 で疑問を出し切る** — 途中で「実はこう解釈してた」は最悪
- **PR はユーザーがマージする** — 作るだけ。マージを急かさない
- **実装の範囲を守る** — Issue に書いてないことを追加実装しない
