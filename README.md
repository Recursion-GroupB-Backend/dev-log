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

```mermeid
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
