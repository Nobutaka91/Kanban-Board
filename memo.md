# 環境構築

Reactプロジェクト作成
```bash
npm create vite
```
- プロジェクト名を書く
- 質問に答えていく
```bash
Select a framework: > React
Select a variant: > TypeScript + SWC

```
dnd-kitインストール
```bash
npm install @dnd-kit/core @dnd-kit/sortable
```

tailwindcssインストール
```bash
npm install -D tailwindcss postcss autoprefixer
```
tailwindcssの設定ファイルを作成
```bash
npx tailwindcss init -p
```
- init : 基本設定ファイルを作成
  - tailwind.config.js
- -p : PostCSS設定も作成
  - post.css.config.js

<font color="Pink">上記のコマンドでファイルが自動生成させずエラーとなった場合の解決方法</font>   
[原因]:  Tailwind v4からCLIツールが別パッケージに分離され、明示的インストールが必要になったため  

[解決法] : 
- Tailwind CLIを明示的に
インストールする
  ```bash
  npm install -D tailwindcss-cli
  ```
- 改めてコマンドを実行 (ただし、tailwindcssではなくtailwindcss-cli)
  ```bash
  npx tailwindcss-cli init -p
  ```

[参考記事]

[【Tailwind4】" npx tailwindcss init -p " でファイルが自動生成されずにエラーとなった場合の解決方法](https://zenn.dev/keita09/articles/d92815d3501c7c)


## Tailwind CSS v4 導入（Vite）
- 背景: v4では`init`コマンドが廃止され、`npx tailwindcss init -p`は使わない。
- 手順:
  1) 依存追加（Viteプラグイン方式）
  ```bash
  cd kanban-board
  npm i -D tailwindcss @tailwindcss/vite
  ```
  2) `kanban-board/vite.config.ts` にプラグインを追加
  ```ts
  import { defineConfig } from 'vite'
  import react from '@vitejs/plugin-react-swc'
  import tailwindcss from '@tailwindcss/vite'

  export default defineConfig({
    plugins: [react(), tailwindcss()],
  })
  ```
  3) ベースCSSでTailwindを読み込み（`kanban-board/src/index.css`）
  ```css
  @import "tailwindcss";
  ```
  - 注意: v4ではこれだけで十分。`App.css` 等にある従来の
    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
    は不要なので削除する。
  4) 不要物の整理（任意）
  ```bash
  # ルートの v3 想定のCLIや設定は不要
  cd ..
  npm uninstall -D tailwindcss-cli
  # ルートの `tailwind.config.js` / `postcss.config.js` は v4 では原則不要
  ```
  5) 起動
  ```bash
  cd kanban-board
  npm run dev
  ```
- 参考: https://tailwindcss.com/docs/guides/vite

### 付録: v3 の従来手順を使う場合（`init`が必要なときのみ）
```bash
cd kanban-board
npm i -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
# src/index.css に以下を追加
# @tailwind base; @tailwind components; @tailwind utilities;
```

[補助的なコマンド]  
Tailwindのクラス文字列をマージして、重複や競合を自動的に解決する

```bash
npm install tailwind-merge
```
例 : `p-2 p-4` → `p-4`   
    `text-left text-center` → `text-center`