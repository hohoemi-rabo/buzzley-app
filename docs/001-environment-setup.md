# 001: 環境構築とプロジェクトセットアップ

## 概要
開発環境とデプロイ環境の初期セットアップ

## 優先度
高（必須）

## 依存関係
なし

## タスク一覧

### 開発環境
- [ ] Node.js環境の確認（v18以上）
- [ ] パッケージマネージャーの設定確認
- [ ] 開発サーバーの動作確認 (`npm run dev`)
- [ ] ESLintの動作確認 (`npm run lint`)
- [ ] ビルドの動作確認 (`npm run build`)

### Cloudflare環境
- [ ] Cloudflareアカウントの作成
- [ ] Cloudflare Workers プロジェクトの作成
- [ ] wrangler CLIのインストール
- [ ] wrangler.tomlの設定
- [ ] Cloudflare KVネームスペースの作成（キャッシュ用）
- [ ] 環境変数の設定（CLOUDFLARE_ACCOUNT_ID等）

### デプロイ設定
- [ ] GitHub リポジトリとCloudflareの連携
- [ ] 自動デプロイの設定
- [ ] カスタムドメインの設定（オプション）

## 受け入れ条件
- ローカルで開発サーバーが正常に起動する
- Cloudflare Workersにデプロイできる
- ビルドエラーがない

## 技術メモ
- Next.js 15.5.4 with App Router
- Cloudflare Workers無料プラン: 100,000リクエスト/日
- Cloudflare KV無料プラン: 100,000読み取り/日、1,000書き込み/日

## 参考資料
- [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)
- [Next.js on Cloudflare](https://developers.cloudflare.com/pages/framework-guides/nextjs/)