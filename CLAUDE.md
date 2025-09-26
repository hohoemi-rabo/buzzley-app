# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Development
- `npm run dev` - Start development server at http://localhost:3000
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint for code quality checks

### Testing
No test framework is currently configured. When implementing tests, first check with the user for their preferred testing framework.

## AIツール使い分けガイドライン

### Codex CLI と Claude Code の使い分け
開発状況に応じて適切なAIツールを選択してください：

#### Codex CLI を使用する場面
- **バグ修正の難航時**: 同じバグ修正が3回以上失敗した場合は、`codex exec "バグの詳細分析と解決策の提案"` で深い分析を実施
- **アーキテクチャ設計**: システム全体の設計や大規模なリファクタリング検討時は `codex --search` で最新のベストプラクティスを調査
- **複雑な問題の分析**: 原因不明のエラーや複雑な依存関係の問題は Codex CLI で多角的に分析

#### Claude Code を使用する場面（デフォルト）
- **通常の実装作業**: 機能追加、コンポーネント作成、API実装などの標準的な開発作業
- **コードレビュー**: 既存コードの改善提案やリファクタリング
- **ドキュメント作成**: README、コメント、仕様書の作成・更新
- **簡単なバグ修正**: エラーメッセージが明確で原因が特定しやすいバグの修正

### 実行例
```bash
# バグの深い分析が必要な場合
codex exec "TypeScriptのビルドエラーが解決できない問題を分析して解決策を提示"

# アーキテクチャの相談
codex --search "Next.js App Router で大規模アプリケーションのフォルダ構成のベストプラクティス"

# 通常の実装はClaude Codeで継続
# （このファイルを読んでいるのがClaude Codeの場合）
```

## Architecture

This is a **Next.js 15.5.4** application using the **App Router** architecture with TypeScript and Tailwind CSS v3.

### Key Technologies
- **Next.js App Router**: All pages and layouts are in `src/app/`
- **React 19.1.0**: Latest React with server components support
- **TypeScript**: Full type safety with strict mode enabled
- **Tailwind CSS 3.4**: Utility-first CSS (recently downgraded from v4 for compatibility)

### Project Structure
- `src/app/` - App Router pages, layouts, and styles
- `src/app/layout.tsx` - Root layout with font loading and global HTML structure
- `src/app/page.tsx` - Home page component
- `src/app/globals.css` - Global styles with Tailwind directives and CSS variables
- Path alias: `@/*` maps to `src/*` for clean imports

### Styling Approach
- Tailwind CSS with dark mode support via CSS custom properties
- Geist font family (Sans and Mono) loaded via `next/font/local`
- Mobile-first responsive design
- CSS variables defined in `:root` and `.dark` for theming

### Important Notes
- No test framework currently configured - ask user before adding tests
- ESLint configured with Next.js recommended rules
- TypeScript strict mode is enabled
- The project uses modern ES modules (.mjs config files)

## Next.js App Router ベストプラクティス

### コンポーネント構成
- **Server Components をデフォルトで使用**: `"use client"` ディレクティブは必要な場合のみ追加
- **Client Components は最小限に**: インタラクティブな機能が必要な部分のみクライアントコンポーネント化
- **コンポーネントの粒度**: Server/Client境界を意識して適切に分割

### ファイル構成
```
src/app/
├── (routes)/              # ルートグループで論理的に整理
├── _components/           # プライベートコンポーネント（ルーティング対象外）
├── api/                   # API Routes
├── layout.tsx             # 共通レイアウト
├── page.tsx              # ページコンポーネント
├── loading.tsx           # ローディングUI
├── error.tsx             # エラーバウンダリ
└── not-found.tsx         # 404ページ
```

### データフェッチング
- **Server Components でのfetch**: キャッシュとrevalidation設定を活用
  ```typescript
  // スタティック（デフォルト）
  fetch(url)

  // リバリデーション付き
  fetch(url, { next: { revalidate: 3600 } })

  // 動的
  fetch(url, { cache: 'no-store' })
  ```
- **Parallel Data Fetching**: 複数のデータフェッチは並列実行
- **Streaming とSuspense**: 段階的なページレンダリング

### パフォーマンス最適化
- **Dynamic Imports**: 必要に応じて`dynamic()`を使用
- **Image Optimization**: `next/image`コンポーネントを使用
- **Font Optimization**: `next/font`でフォントを最適化（既に実装済み）
- **Metadata API**: `generateMetadata`でSEO最適化

### ルーティングパターン
- **Dynamic Routes**: `[param]`、`[...slug]`、`[[...slug]]`の使い分け
- **Route Groups**: `(folder)`で論理的なグループ化（URLに影響なし）
- **Parallel Routes**: `@folder`で複数のページを同時レンダリング
- **Intercepting Routes**: `(.)folder`でモーダルなどの実装

### 状態管理
- **Server State**: Server Componentsでデータフェッチ
- **Client State**: 必要最小限のuseState/useReducer
- **URL State**: searchParamsを活用した状態管理
- **Form Actions**: Server Actionsでフォーム処理

### Server Actions
```typescript
// app/actions.ts
'use server'

export async function createItem(formData: FormData) {
  // サーバーサイドでの処理
  // データベース操作など
  revalidatePath('/items')
}
```

### エラーハンドリング
- **error.tsx**: 各ルートレベルでエラーバウンダリ設定
- **not-found.tsx**: 404エラーのカスタマイズ
- **loading.tsx**: Suspenseベースのローディング状態

### キャッシング戦略
- **Request Memoization**: 同一リクエスト内での重複fetch最適化
- **Data Cache**: fetchレスポンスのキャッシュ
- **Full Route Cache**: 静的ルートのキャッシュ
- **Router Cache**: クライアントサイドのナビゲーションキャッシュ

## プロジェクト要件

### サービス概要
**Twitchバズり配信検索サービス** - フォロワー数は少ないが視聴者数が多い「バズっている配信」を発見できる検索サービス

### 主要機能（MVP）

#### 検索・フィルタ機能
- **ゲームタイトル検索**: 部分一致検索による配信ゲーム絞り込み
- **視聴者数フィルタ**: 範囲指定（最小値〜最大値）
- **フォロワー数フィルタ**: 範囲指定（最小値〜最大値）
- **バズり度**: 視聴者数 ÷ フォロワー数の自動算出（％表示）

#### 表示機能
- 配信者名（Twitchへのリンク付き）
- リアルタイム視聴者数
- 現在のフォロワー数
- バズり度（％表示）
- 配信タイトル
- サムネイル画像
- 無限スクロール（10件ずつ追加読み込み）
- リアルタイム更新（30秒〜1分間隔）

#### ソート機能
- バズり度順（デフォルト）
- 視聴者数順
- フォロワー数順

### バズり判定ロジック

#### 対象配信者
フォロワー数: 1人〜5,000人

#### 判定基準
**新人層（フォロワー 1〜1,000人）**
- 視聴者数 ÷ フォロワー数 ≥ 20% AND 視聴者数 ≥ 20人

**中堅層（フォロワー 1,001〜5,000人）**
- 視聴者数 ÷ フォロワー数 ≥ 10% AND 視聴者数 ≥ 100人

### 技術要件

#### API連携
- **Twitch API**: 無料プラン利用
  - GET /streams - ライブ配信情報
  - GET /users - ユーザー情報（フォロワー数等）
  - GET /games - ゲーム情報
  - GET /search/channels - チャンネル検索

#### Rate Limit対策
- Cloudflare KVを利用したキャッシュ（30秒〜1分）
- リクエストのバッチ処理
- エラー時の指数バックオフ実装

#### デプロイ環境
- **Cloudflare Workers**: エッジコンピューティング
- **Cloudflare KV**: キャッシュストレージ

### UI/UXガイドライン

#### デザイン原則
- **シンプル**: 余計な装飾を排除
- **直感的**: 説明なしで使える設計
- **高速**: 軽快な動作
- **モダン**: 最新のWebデザイントレンド

#### カラースキーム
- プライマリ: Twitch紫（#9146FF）
- セカンダリ: グレー系
- アクセント: 緑（バズり表示用）
- 背景: 白（将来ダークモード対応）

### 開発フェーズ

**Phase 1 (MVP)** - 現在
- 基本的な検索・フィルタ機能
- バズり度計算・表示
- 無限スクロール
- Google Analytics導入

**Phase 2**
- ダークモード対応
- 言語フィルタ（日本語配信）
- UI改善

**Phase 3**
- Cloudflare D1データベース導入
- 検索履歴機能
- お気に入り機能

**Phase 4**
- 認証機能
- 配信者ダッシュボード
- 成長率分析

### パフォーマンス要件
- レスポンスタイム: 検索実行から結果表示まで5秒以内
- 同時接続数: 100ユーザーを想定
- 可用性: 99%以上

## チケット管理

### チケットファイル
`/docs`ディレクトリに機能ごとのチケットファイルが格納されています。各ファイルは連番（001〜）で管理されています。

### Todo管理ルール
各チケット内のタスクは以下の形式で管理します：
- `- [ ]` : 未完了のタスク
- `- [x]` : 完了したタスク

タスクを完了したら、必ず`[ ]`を`[x]`に更新してください。

### 主要チケット一覧
- 001: 環境構築とプロジェクトセットアップ
- 002: Twitch API統合
- 003: 検索・フィルタ機能
- 004: ソート機能
- 005: 配信情報表示コンポーネント
- 006: 無限スクロール実装
- 007: リアルタイム更新機能
- 008: バズり判定ロジック
- 009: UIデザインシステム
- 010: ランディングページ
- 011: パフォーマンス最適化
- 012: エラーハンドリング
- 013: アナリティクス導入