# 003: 検索・フィルタ機能

## 概要
配信検索とフィルタリング機能の実装

## 優先度
高（MVP必須）

## 依存関係
- 002-twitch-api-integration.md

## タスク一覧

### 検索フォームコンポーネント
- [ ] 検索フォームUIコンポーネント作成 (`src/components/SearchForm.tsx`)
- [ ] ゲームタイトル検索入力フィールド
- [ ] 視聴者数範囲入力（最小値・最大値）
- [ ] フォロワー数範囲入力（最小値・最大値）
- [ ] 検索ボタン
- [ ] リセットボタン

### 検索ロジック実装
- [ ] ゲームタイトルの部分一致検索
- [ ] 視聴者数によるフィルタリング
- [ ] フォロワー数によるフィルタリング
- [ ] 複合条件での検索

### API Route実装
- [ ] `/api/search` エンドポイント作成
- [ ] クエリパラメータのバリデーション
- [ ] Twitch API呼び出しの最適化
- [ ] エラーハンドリング

### 状態管理
- [ ] 検索条件の状態管理（URLパラメータ）
- [ ] 検索結果の状態管理
- [ ] ローディング状態の管理
- [ ] エラー状態の管理

### バリデーション
- [ ] 入力値の型チェック
- [ ] 数値範囲の妥当性チェック
- [ ] エラーメッセージの表示

## 受け入れ条件
- ゲームタイトルで配信を検索できる
- 視聴者数・フォロワー数で絞り込める
- 複数条件を組み合わせて検索できる
- 無効な入力値に対してエラーが表示される

## 技術メモ
- URL SearchParamsを使用して検索条件を管理
- Server Actionsでフォーム処理
- zodでバリデーション

## 参考資料
- [Next.js Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)
- [URL State Management](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#using-the-native-history-api)