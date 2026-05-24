# codemica.net

[Codemica](https://codemica.net) — Software studio for daily craft.

個人開発スタジオ **Codemica** の公式ホームページです。
`https://codemica.net` で公開しています（GitHub Pages + カスタムドメイン）。

---

## このリポジトリの中身

シンプルな静的 1 ファイル構成。

```
codemica.net/
├── index.html          # トップページ（CSS インライン、JS なし）
├── CNAME               # カスタムドメイン設定（codemica.net）
├── README.md           # このファイル
└── assets/
    └── images/
        ├── sprout-ripple.png   # メインロゴ（金色双葉 + 波紋）
        ├── sprout-faded.png    # Coming Soon プレースホルダー
        ├── growth-path.png     # Vision セクションの成長グラフ
        ├── book-pen.png        # Blog セクションの本イラスト
        ├── waves-soft.png      # フッターの波装飾
        ├── weavelog-icon.png   # Weavelog アプリアイコン
        ├── icon.png            # Codemica 公式アイコン（Favicon）
        ├── header.png          # SNS 用ヘッダー画像（汎用）
        └── （将来用ジェネリック素材 11 個）
```

ローカルで見るには `open index.html` だけで OK。
ビルドステップなし、依存パッケージなし。

---

## 制作プロセス（2026-05-24 完成）

このサイトは **AI ツール 3 つを連携** して 1 日で構築しました。
将来 2 作目以降のアプリページを作る時のリファレンスとして残しておきます。

### 使ったツール

| ツール | プラン | 役割 |
|---|---|---|
| **ChatGPT Plus** | $20/月 | デザインモック画像生成 + 素材グリッド生成（GPT Image 2） |
| **Claude Code** | Max $220/月 | ファイル操作、コード書き込み、Git 管理、最終 handoff 取り込み |
| **Claude Design** | Max $220/月 | HTML/CSS の本実装、UI ベースのコメント / 微調整 |

### フェーズごとの流れ

**フェーズ 1: モック画像生成（ChatGPT Plus / GPT Image 2）**

「Codemica のランディングページモック、ブランドアイデンティティ、セクション構成、ビジュアルスタイル...」を細かく指示してモック画像を生成。
1 ページ完結のロングスクロール構成（Hero / About / Products / Tech Stack / Vision / Blog / Press / Contact）を確定。

**フェーズ 2: 素材グリッド生成（ChatGPT Plus / GPT Image 2）**

モック画像の中で使われている素材（双葉・波紋・グラデブロブ・装飾線等）を **3x3 グリッド 2 枚 = 18 個** として一括生成。
スタイル統一性を最大限保つため「前のグリッドと完全に同じスタイルで」と指示。

**フェーズ 3: Claude Code で骨組み実装（後で破棄）**

Python + Pillow で 3x3 グリッドを分割 + 透過処理し、`parts/` フォルダに 18 個の用途別 PNG を準備。
Tech Stack のロゴは [Simple Icons](https://simpleicons.org/) から公式 SVG を取得。
セマンティック HTML + CSS Variables で初版を実装したが、「シンプルすぎて寂しい」というフィードバックで方向転換。

**フェーズ 4: Claude Design で本実装**

[claude.ai/design](https://claude.ai/design) に素材 PNG とモック画像をアップロード。
「素材をもとに LP を忠実に作ってください」の一声で、HTML/CSS インラインの完成度の高いプロトタイプを生成。
その後、Comments / Edit / Tweaks 機能で部分修正:
- 素材 PNG の白背景を透過化
- Coming Soon ラベルを `white-space: nowrap` で 1 行表示
- About セクションのイラストを `mix-blend-mode: multiply` で背景に馴染ませる
- イラストの彩度・コントラストを微調整

**フェーズ 5: Claude Code に handoff bundle で取り込み**

Claude Design の右下「Implement with Claude Code」から発行された URL（Anthropic 公式 API、`api.anthropic.com/v1/design/h/...`）を Claude Code に渡す。
Claude Code が WebFetch でデザインバンドル（tar.gz、`README.md` + `chats/` + `project/`）をダウンロード、解凍、内容を解釈。
ローカルリポジトリの構造に合わせて配置 + **Codemica の実情に合わせた 11 個の修正** を一括適用:

1. Weavelog 説明文を「日々を綴り、応援し合う日記 SNS」に修正（Claude Design は推測で書いていた）
2. Weavelog アプリアイコンを実物 PNG に差し替え（Claude Design は雰囲気 SVG）
3. メアドを `codemica.app@gmail.com` に修正
4. X / note / App Store のリンク URL を実 URL に
5. © を 2026 に
6. meta / OGP / Twitter Card / Favicon 追加
7. `<img>` の alt 属性を意味あるテキストに
8. `aria-labelledby` / `aria-hidden` などアクセシビリティ強化
9. `prefers-reduced-motion` 対応
10. ホバーアニメーション微追加
11. 商標問題のあるプレースホルダー画像（旧 Twitter ロゴ）を削除

これで完成。`git push` で main へ直 push、GitHub Pages が自動デプロイ。

---

## デプロイ

ブランチ戦略は **main 直 push**（個人開発、PR なし）。

```bash
git add -A
git commit -m "feat: 〜"
git push origin main
```

GitHub Pages が main の更新を検知して自動デプロイ。数分後に `https://codemica.net` で反映されます。

---

## 関連リンク

- 🌱 **Weavelog**（最初のプロダクト、日記 SNS）: [weavelog.net](https://weavelog.net)
- ✍️ **note**（開発ブログ）: [note.com/codemica_app](https://note.com/codemica_app)
- 🐦 **X**（運営者発信）: [@codemica_app](https://x.com/codemica_app)
- 📧 **Contact**: codemica.app@gmail.com

---

## License

Code: MIT
Assets (画像 / イラスト): All rights reserved © 2026 Codemica

---

_最終更新: 2026-05-24_
