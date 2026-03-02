# allInHead

**あなたの頭の中、ぜんぶここに。**

allInHead は、AIをあなたの「わかってる相棒」にするためのリポジトリ構造です。

あなたの知識・判断基準・文脈をリポジトリに蓄積することで、AIは毎回ゼロから説明されなくても、前提を理解したパートナーとして動けるようになります。

## どういう仕組み？

```
allInHead-starter/
├── CLAUDE.md              ← AIの人格定義 + 行動ルール（ここが本体）
├── me/                    ← AIの記憶
│   ├── identity.md        ← 自分は誰か
│   ├── core-memory.md     ← 大切な記憶
│   ├── notepad.md         ← セッション間の引き継ぎ
│   ├── diary/             ← 日記
│   └── work-log/          ← 作業記録
├── world/                 ← あなたの頭の中（知識・情報・判断基準）
│   └── about-you.md       ← あなた自身の情報
├── outputs/               ← 成果物
└── .claude/
    └── agents/
        └── kage.md        ← カゲ（セキュリティ・リスク評価チームメイト）
```

- **`world/`** にあなたの知識を入れる → AIがあなたの文脈を理解する
- **`me/`** にAIが自分の記憶を書く → セッションをまたいで成長する
- **`CLAUDE.md`** でAIの振る舞いを定義する → 毎回同じパートナーが立ち上がる

## はじめ方

### 1. このリポジトリをフォークまたはクローン

```bash
git clone https://github.com/nagi-y/allInHead-starter.git
cd allInHead-starter
```

### 2. CLAUDE.md をカスタマイズ

`CLAUDE.md` を開いて、AIの名前・性格・口調を自分好みに書き換える。

### 3. world/ にあなたの情報を入れる

`world/about-you.md` を編集して、あなた自身のことを書く。プロジェクト情報、判断基準、好みなど。

### 4. Claude Code で開く

```bash
claude
```

AIがあなたの文脈を理解した状態で起動します。セッションを重ねるごとに `me/` の記憶が蓄積され、より深いパートナーになっていきます。

## 動作環境

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI または Web)

## ライセンス

MIT
