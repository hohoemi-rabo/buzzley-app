# 004: ソート機能

## 概要
検索結果のソート機能実装

## 優先度
高（MVP必須）

## 依存関係
- 003-search-filter-feature.md

## タスク一覧

### ソートUI実装
- [ ] ソート選択コンポーネント作成 (`src/components/SortSelector.tsx`)
- [ ] ソートオプション（バズり度、視聴者数、フォロワー数）
- [ ] デフォルト：バズり度順の設定
- [ ] ソート方向切り替え（昇順/降順）

### ソートロジック実装
- [ ] バズり度順ソート（降順デフォルト）
- [ ] 視聴者数順ソート
- [ ] フォロワー数順ソート
- [ ] ソート状態のURL管理

### パフォーマンス考慮
- [ ] クライアントサイドソートの実装
- [ ] 大量データ対応（仮想スクロール検討）

## 受け入れ条件
- 3種類のソート基準で並び替えができる
- ソート状態がURLに保持される
- ソート切り替えが高速に動作する

## 技術メモ
- URLのsortパラメータで管理
- 例: `?sort=buzz_rate_desc`

## 参考資料
- [Array.prototype.sort()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)