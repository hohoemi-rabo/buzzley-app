# 007: リアルタイム更新機能

## 概要
配信情報の定期的な自動更新（30秒〜1分間隔）

## 優先度
中（MVP推奨）

## 依存関係
- 005-stream-display-component.md
- 002-twitch-api-integration.md

## タスク一覧

### 自動更新実装
- [ ] setIntervalまたはsetTimeoutベースの更新ロジック
- [ ] 30秒〜1分の更新間隔設定
- [ ] バックグラウンドタブ時の更新停止
- [ ] フォーカス復帰時の即時更新

### 更新処理
- [ ] 差分更新の実装（全置換ではなく）
- [ ] 視聴者数のアニメーション更新
- [ ] 新規配信の追加通知
- [ ] 終了配信の削除

### ユーザー制御
- [ ] 自動更新のON/OFF切り替え
- [ ] 手動更新ボタン
- [ ] 最終更新時刻の表示
- [ ] 更新中インジケーター

### パフォーマンス考慮
- [ ] 最小限のDOM更新
- [ ] RequestIdleCallbackの活用
- [ ] メモリリーク対策

## 受け入れ条件
- 30秒〜1分間隔で自動更新される
- バックグラウンドタブで更新が停止する
- ユーザーが更新をコントロールできる
- 更新時にちらつきがない

## 技術メモ
- Page Visibility API使用
- React.memoで不要な再レンダリング防止
- WebSocketは将来的に検討

## 参考資料
- [Page Visibility API](https://developer.mozilla.org/ja/docs/Web/API/Page_Visibility_API)
- [requestIdleCallback](https://developer.mozilla.org/ja/docs/Web/API/Window/requestIdleCallback)