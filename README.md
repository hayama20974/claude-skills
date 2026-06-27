# Claude Skills — App Launch Kit

> Ship your app's launch assets in one conversation.
> アプリのリリース素材を「対話だけ」で一気通貫に出荷する [Claude](https://claude.com/claude-code) スキル。

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

個人開発でいちばん消耗するのは、機能ではなく **公開直前の素材づくり**です。
アイコンの全密度書き出し、1024×500ぴったりのフィーチャーグラフィック、文字数制限のあるストア文章、そして弁護士でもないのに書かされるプライバシーポリシー。
このスキルは、それらを **一つのブランドで・正しい順番で** AIに作らせます。

## What it does / 何をするか

`app-launch-kit` は、リリース素材5点を **アイコン → LP → フィーチャーグラフィック → ストア文章 → プライバシーポリシー** の順で制作します。

```
①アイコン ──→ ②LP ──→ ③グラフィック ──→ ④ストア文章 ──→ ⑤ポリシー ──→ 公開
   └─ ここで決めた1色とモチーフが、下流すべてのデザイントークンになる ─┘
```

順番に意味があります。アイコンで確定した色・角丸・モチーフが下流すべての成果物に流れ込み、LPで磨いたコピーがストア文章の骨格になる。だから後から「色を合わせにいく」作業がゼロになります。

## 設計の核心 / Three design ideas

このスキルの価値は「5点を作れること」ではなく、自動化が破綻しがちな箇所を **3つの判断**で回避していることです。

1. **色は下流へ「流れる」** — 最初にアイコンを確定し、その配色・角丸・モチーフをCSS変数とPillowの描画パラメータとして後続に引き継ぐ。
2. **スクリプトはスキルに同梱せず、毎回その場で書き起こす** — アイコン/グラフィックを描く Pillow スクリプトは確定デザインに合わせて都度生成する。スキルに古いコードを抱えず、ネットワーク無効のビルド環境でも完結する。公開（アップロード）の自動化は環境差が大きいのでスキルの外。
3. **AIに「品質の床」を踏ませる** — 生成しっぱなしにしない。目視確認・フォント実在確認(`fc-list`)・未確定事項のTODO化で、AIの“それっぽい嘘”を構造で潰す。

## Install / 導入

Claude Code のスキルとして読み込みます。

```bash
git clone https://github.com/hayama20974/claude-skills.git
cp -r claude-skills/skills/app-launch-kit ~/.claude/skills/
```

これで [`skills/app-launch-kit/SKILL.md`](skills/app-launch-kit/SKILL.md) が `~/.claude/skills/app-launch-kit/SKILL.md` に入ります。あとは「アプリのリリース素材を作って」「アイコンを作って」のように話しかければ発火します。

## Configure / 自分の環境に合わせる

このスキルは公開用のジェネリック版です。使う前に以下を自分の値へ置き換えてください（詳細は SKILL.md 末尾の表）。

| プレースホルダー | 意味 |
|---|---|
| `com.<your-namespace>` | パッケージ名の固定空間（例 `com.example`） |
| `<your-blog-url>` | LP/ポリシーの公開先サイト |
| フォームサービス | リリース前LPのメール収集（Formspree 等） |
| 公開手段 | 静的ファイルの設置方法（FTP / S3 / Netlify / GitHub Pages 等） |

## Notes / 注意

- プライバシーポリシーは **雛形**です。公開前に専門家の確認を受け、Google Play「データセーフティ」の回答と一致させてください。
- 価格・効果などの未確定事項は、AIに断定させない設計にしています。

## 解説記事 / Article

設計の背景と使い方は、開発ブログで解説しています → **[AIバイブコーディングTips](https://www.meclgg.com/aitips/)**

## License

[MIT](LICENSE)
