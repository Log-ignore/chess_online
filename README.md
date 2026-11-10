# Chess Online

オンライン対人戦専用のシングルページチェスアプリです。タクティクス、CPU対戦、ローカル対局履歴・分析などの機能は含めていません。

## 機能
- ルーム作成・ルームID発行
- パスワードによる入室チェック
- 白（作成者）・黒（参加者）の2人対局
- Firebase Realtime Database による局面・棋譜のリアルタイム同期
- 持ち時間（1+0 / 3+0 / 3+2 / 5+0 / 5+3 / 10+0 / 15+0 / 30+0）
- 定型文チャット、自由入力チャット
- 投了、チェックメイト、ステイルメイト、戦力不足、時間切れ

## Firebase セットアップ
1. Firebase Console でプロジェクトを作成し、Web アプリを追加します。
2. Authentication の Anonymous provider を有効にします。
3. Realtime Database を作成し、Database URL を取得します。
4. Web アプリ設定をアプリの「接続設定」に入力して接続します。
5. Realtime Database Rules を設定します。開発用の最小例：

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": "auth != null",
        ".write": "auth != null"
      }
    }
  }
}
```

このルールは認証済みユーザー全員に読み書きを許可する簡易設定です。公開運用では不十分です。Firebase App Check と、サーバー側でパスワード・参加人数・手番・時計・着手の妥当性を検証する Cloud Functions 等を追加し、Database Rules もルーム参加者単位に制限してください。

## パスワードに関する注意
現在の実装ではパスワードを SHA-256 ハッシュ化して照合しますが、照合はクライアント側です。Realtime Database を直接読む権限を持つユーザーはハッシュを取得できるため、これは強固なサーバー認証ではありません。実際の公開運用ではサーバー側検証が必要です。

## 公開
GitHub Pages を有効化し、`main` ブランチの `/ (root)` を公開元に指定してください。Firebase 設定はブラウザ内の localStorage に保存されます。
