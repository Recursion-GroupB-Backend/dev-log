# 開発議事録 (11/25)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### プロジェクトの概要
- オンラインチャットアプリケーションを開発
- CLIの対話形式を想定

### 今後の開発想定
- stage1(必須)
  - 各自で開発して基本的なTCPとUDPについて学ぶ
  - 良さげなメンバーのコードを採用してstage2へ進む
  - 理想は3~4日程度で達成したい
- stage2(必須)
  - ここから本格的なチーム開発
  - 必要であれば各種設計なども行う
- stage3(追加)
  - 時間が余れば着手

### MTGスケジュール
- 火曜日 20:00
- 木曜日 20:00
- 土曜日 10:00

### gitでの開発について
- Git-flowを採用
- 承認は最低1つ必要(できる限り全員レビューを心がける)
- マージはPR発行者が行う(mainブランチへのマージはMTGとかでやった方が良いかも？)
- PRする際は必ず最新developの内容を反映させてから行う(本番マージでコンフリクトを起こさないため)
- [PRはこちらのフォーマットを参照](https://github.com/recursion-team-v/team-v-devlog/blob/main/github_tutorial.md)


### 開発ツール
- flake8をコードフォーマッターとして使用
- 開発環境はUbuntuで統一
- Pythonバージョン3.1以上


## タスク
**[@sei](https://github.com/takatokawazu)**
- [#issue1 stage1開発](https://github.com/Recursion-GroupB-Backend/Online-Chat-Messenger/issues/1)

**[@タカトさん](https://github.com/takatokawazu)**
- [#issue1 stage1開発](https://github.com/Recursion-GroupB-Backend/Online-Chat-Messenger/issues/1)

**[@koyuさん](https://github.com/takatokawazu)**
- [#issue1 stage1開発](https://github.com/Recursion-GroupB-Backend/Online-Chat-Messenger/issues/1)

## 次回ミーティング11/28(火) 20:00~


# 開発議事録 (11/28)

## 決めたこと
### アクティビティ図
処理の流れ<br>
1. ユーザー名入力
2. CLI or GUIの選択
3. チャット作成or参加の選択
4. これまでの入力値をTCPで送信
5. サーバ側は受け取った情報を元にユーザー・チャットルームを作成
6. レスポンスコードを送信
7. UDP接続でチャットスタート

```mermaid
graph TD
    A[スタート] --> B[名前入力]
    B --> C[CLI]
    B --> D[GUI]
    B --> Z[プログラム終了]
    C -- TCP通信 --> E[チャットルーム作成]
    C -- TCP通信 --> F[チャットルーム参加]
    E -- UDP通信 --> G[チャット]
    F --> H[ルーム名入力]
    H -- UDP通信 --> G
    G --> Z[チャット終了]
```

## タスク
**[@sei](https://github.com/takatokawazu)**
- クラス図の作成

**[@タカトさん](https://github.com/takatokawazu)**
- stage1開発とコードレビュー

**[@koyuさん](https://github.com/takatokawazu)**
- stage1のプルリクをあげる

## 次回ミーティング11/30 21:00





# 開発議事録 (11/30)

## 決めたこと
### アクティビティ図
処理の流れ<br>
1. ユーザー名入力
2. CLI or GUIの選択
3. チャット作成or参加の選択
4. これまでの入力値をTCPで送信
5. サーバ側は受け取った情報を元にユーザー・チャットルームを作成
6. レスポンスコードを送信
7. UDP接続でチャットスタート

### チャットルームの作成

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Client: ユーザー名入力、
    Client->>Client: チャットルーム名入力、
    Client->>Server: TCP接続要求（ユーザー名、ルーム情報、リクエストコード）
    Server->>Server: ユーザーとトークン生成
    Server->>Server: チャットルームの生成
    Server->>Client: TCP応答（トークン、ステータス）
    Client->>Server: UDP接続要求（トークン）
    Server->>Server: トークン検証とルームへの追加
    Server->>Client: UDP応答（接続確認）
    loop メッセージ交換
        Client->>Server: UDPメッセージ送信
        Server->>Client: UDPメッセージ中継
    end
    Client->>Server: チャット終了（切断要求）
    Server->>Server: ユーザーとトークン削除
```

### チャットルーム参加

```mermaid
sequenceDiagram
    participant Client
    participant Server

    Client->>Client: ユーザー名入力、
    Client->>Client: チャットへ参加、
    Client->>Client: 参加するルーム名の入力、
    Client->>Server: TCP接続要求（ユーザー名、ルーム情報、リクエストコード）
    Server->>Server: ユーザーとトークン生成
    Server->>Server: チャットルームの生成
    Server->>Client: TCP応答（トークン、ステータス）
    Client->>Server: UDP接続要求（トークン）
    Server->>Server: トークン検証とルームへの追加
    Server->>Client: UDP応答（接続確認）
    loop メッセージ交換
        Client->>Server: UDPメッセージ送信
        Server->>Client: UDPメッセージ中継
    end
    Client->>Server: チャット終了（切断要求）
    Server->>Server: ユーザーとトークン削除
```

## 直近目標
- 要件に沿ったカスタムTCPの開発
- まずはTCP通信でチャットルームを作成する

## タスク
**[@sei](https://github.com/takatokawazu)**
- TCPレスポンス作成
- ルーム作成(仮データ)

**[@タカトさん](https://github.com/takatokawazu)**
- チャットルームクラス作成

**[@koyuさん](https://github.com/takatokawazu)**
- カスタムTCP作成(機能要件要確認)

## 次回ミーティング12/2 10:00

# 開発議事録 (12/2)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### 完成物ミーティングの日程調整
- 12/16日の11:30分

### TCP通信について
-> bodyに含める値はどうするか？<br>
RoomNameとOperationPayloadに分けて、payloadの方にルーム名とユーザー名を含めるようにする
![image](https://github.com/Recursion-GroupB-Backend/dev-log/assets/69625901/44a758e0-2fed-4b64-93c0-ac9295b088aa)

### 直近目標
タカトさんのソースを反映して、バックエンド側でチャット生成処理を入れることでTCP通信は完了。
その後はissueを追加していきstage2の要件を潰していく。


## タスク
**[@sei](https://github.com/takatokawazu)**
- サーバ側でのレスポンス作成
- チャットルーム生成処理の実行

**[@タカトさん](https://github.com/takatokawazu)**
- チャットルームクラスの雛形作成

**[@koyuさん](https://github.com/takatokawazu)**
- クライアント側のTCPリクエストの生成

## 次回ミーティング12/5(火) 20:00~

# 開発議事録 (12/5)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### やったこと
- sei
  - チャットの参加・作成
- タカトさん
  - チャットルーム作成
  - PRレビュー
- koyuさん
  - カスタムTCP作成
  - UDP切り替え処理
 
## カスタムTCPについて
- 一旦仕様書通りで進める

### stage2完了
- メッセージの中継システム
- ユーザーの削除システム

## タスク
**[@sei](https://github.com/takatokawazu)**
- メッセージの中継システム


**[@タカトさん](https://github.com/takatokawazu)**
- userの削除システム

**[@koyuさん](https://github.com/takatokawazu)**
- クライアント側のTCPリクエストの生成
- GUI雛形作成

## 次回ミーティング12/7(木) 20:00~


# 開発議事録 (12/7)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### やったこと
- sei
  - ユーザー認証
  - UDP通信のレシーブ処理
- タカトさん
  - ユーザーの削除処理
- koyuさん
  - GUIの雛形作成
  - UDPのリクエスト処理
 
## カスタムUDPについて
- 使用通りの形で受信する

## 疑問点解消
- TkinterはClientクラスを継承すればいける
- トークンサイズが1バイトに収まらないのでgenarate_tokenでサイズを小さくして対応
- UDPの受信側はKoyuさんの送信方法と揃える

### stage2完了
- メッセージの中継システム(もうちょい)
- ユーザーの削除システム(もうちょい)

## タスク
**[@sei](https://github.com/takatokawazu)**
- メッセージの中継システム


**[@タカトさん](https://github.com/takatokawazu)**
- userの削除システム
- 時間切れユーザーの削除

**[@koyuさん](https://github.com/takatokawazu)**
- UDPのリクエスト処理
- GUI雛形作成

## 次回ミーティング12/9(土) 10:00~

# 開発議事録 (12/9)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### やったこと
- sei
  - サーバ側のUDPレスポンス処理
  - PRして承認済み
  - TCP通信(クライアント)のレビュー
- タカトさん
  - ユーザーの削除処理(関数)
  - アクティブユーザーチェック(関数)
- koyuさん
  - seiの修正依頼の対応
 

## UDPのレスポンス構成
- ヘッダー
  -user_name_size(1バイト)
  -message_size(1バイト)
-body
  -user_name + message

### TODO
- hostユーザーが退出した時の処理
  - 閉じられたことをメンバーに通知
  - 退出したユーザーをChatRoomクラスのusersから削除
  - チャットルームを閉じる
- 通常のユーザーが退出した時の処理
  - 退出したユーザーをChatRoomクラスのusersから削除
  - 退出したことを他のメンバーに通知
- 非アクティブユーザーの監視
  - 規定時間動きがないユーザーは削除
  - 退出したことを他のメンバーに通知


## 次回ミーティング12/12(土) 20:00


# 開発議事録 (12/12)

## 開発期間
- 2022年11月25日(土) 〜 2022年12月9日(土)

## 決めたこと
### やったこと
- sei
  - メッセージ1件目がリレーされないバグをを修正
  - タイムアウト処理のサンプル作成
- タカトさん
  - ユーザーの削除処理
  - アクティブユーザーチェック
- koyuさん
  - 既存のserverとclientを使いGUIベースのチャット機能を実装

### TODO
- sei
  - パスワード認証
- koyuさん
  - GUI側の開発と残りのイシューを潰す
- タカトさん
  - host削除時の処理


## 次回ミーティング12/14(木) 20:00


# 開発議事録 (12/15)

## 決めたこと
### やったこと
- sei
  - README作成
  - DEMO作成
- タカトさん
  - ユーザーの削除処理
  - コンフリクト修正
- koyuさん
  - GUIのバグ修正
  - コンフリクト修正
 
一通りの要件は達成できたのでGUIの配布などができれば良いなと思う。

### 動作確認
- 機能要件のチェック
- 各自でデバッグ作業

