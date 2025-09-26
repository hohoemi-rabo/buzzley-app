# 006: 無限スクロール実装

## 概要
検索結果の無限スクロール（10件ずつ追加読み込み）機能の実装

## 優先度
中（MVP推奨）

## 依存関係
- 005-stream-display-component.md
- 003-search-filter-feature.md

## タスク一覧

### 無限スクロール実装
- [ ] Intersection Observer APIの実装
- [ ] スクロールトリガーコンポーネント作成
- [ ] ページネーション状態管理
- [ ] 10件ずつのデータフェッチ

### ローディング処理
- [ ] ローディングインジケーター表示
- [ ] スケルトンスクリーン実装
- [ ] エラー時のリトライボタン
- [ ] 全件読み込み完了メッセージ

### パフォーマンス最適化
- [ ] 仮想スクロール検討（大量データ時）
- [ ] メモリ管理（古いデータの破棄）
- [ ] スクロール位置の保持

### 状態管理
- [ ] 現在のページ番号管理
- [ ] hasMore フラグ管理
- [ ] 読み込み中フラグ管理
- [ ] エラー状態管理

## 受け入れ条件
- スクロールで自動的に追加読み込みされる
- ローディング中の表示がある
- 最後まで読み込んだら完了メッセージが表示される
- エラー時にリトライできる

## 技術メモ
- React Hook: `useInfiniteQuery` または自作Hook
- Intersection Observer API使用
- デバウンス処理で過剰なAPI呼び出しを防ぐ

## 参考資料
- [Intersection Observer API](https://developer.mozilla.org/ja/docs/Web/API/Intersection_Observer_API)
- [React Infinite Scroll Components](https://github.com/ankeetmaini/react-infinite-scroll-component)