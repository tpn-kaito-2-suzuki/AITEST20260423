# 要件定義ドキュメント

## はじめに

本ドキュメントは、チーム向けタスク管理Webアプリケーション「Team Task Manager」の要件を定義します。
本システムは内部ツールとして、認証なしで利用できるシンプルな構成とします。
プロジェクト・サブプロジェクト・タスクを階層的に管理し、「誰が・何を・いつまでに」を一目で把握できることを目的とします。

## 用語集

- **System**: Team Task Manager 全体のWebアプリケーション
- **Project**: 複数のサブプロジェクトをまとめる最上位の管理単位
- **SubProject**: Project に属する中間管理単位。タスクを内包する
- **Task**: タイトル・担当者名・期限・ステータス・説明・メモ・関連資料を持つ作業単位
- **Assignee_Name**: Task に設定された担当者のテキスト名称
- **Member**: Project に登録された担当者候補の名前リスト
- **Due_Date**: Task の完了期限日
- **Status**: Task の進捗状態（未着手 / 進行中 / 完了）
- **Task_Service**: Task の作成・更新・削除を担うサービスコンポーネント
- **Project_Service**: Project・SubProject の作成・更新・削除を担うサービスコンポーネント

---

## 要件

### 要件 1: プロジェクト一覧表示

**ユーザーストーリー:** チームメンバーとして、すべてのプロジェクトを一覧で確認したい。そうすることで、どのプロジェクトが存在するかを素早く把握できる。

#### 受け入れ基準

1. WHEN ユーザーがアプリケーションを開いたとき、THE System SHALL すべての Project を一覧表示する
2. THE System SHALL 各 Project のタイトルと説明を一覧上に表示する
3. WHEN Project が1件も存在しないとき、THE System SHALL 「プロジェクトがありません」というメッセージを表示する
4. WHEN ユーザーが一覧上の Project をクリックしたとき、THE System SHALL そのサブプロジェクト一覧画面へ遷移する
5. WHEN ユーザーがプロジェクトカードにカーソルを当てたとき、THE System SHALL 削除ボタンを表示する
6. WHEN ユーザーが削除ボタンを押したとき、THE System SHALL 「delete」の入力を求める確認モーダルを表示する
7. WHEN ユーザーが「delete」と入力して削除を実行したとき、THE Project_Service SHALL Project とその配下のすべての SubProject・Task を削除する

---

### 要件 2: プロジェクト作成・編集

**ユーザーストーリー:** チームメンバーとして、プロジェクトを作成・編集したい。そうすることで、作業のまとまりを管理できる。

#### 受け入れ基準

1. WHEN ユーザーがプロジェクト名を入力して作成を要求したとき、THE Project_Service SHALL 新しい Project を作成する
2. THE Project_Service SHALL プロジェクト名を 1文字以上 100文字以下で受け付ける
3. IF プロジェクト名が空文字列のとき、THEN THE Project_Service SHALL バリデーションエラーを返す
4. WHEN ユーザーが Project の名前または説明を更新したとき、THE Project_Service SHALL 変更内容を保存し、更新後の Project 情報を返す

---

### 要件 3: 担当者管理

**ユーザーストーリー:** チームメンバーとして、プロジェクトに担当者を登録・削除したい。そうすることで、タスク割り当て時にプルダウンから選択できる。

#### 受け入れ基準

1. WHEN ユーザーがサブプロジェクト一覧画面またはタスク一覧画面の「担当者管理」ボタンを押したとき、THE System SHALL 担当者管理モーダルを表示する
2. WHEN ユーザーが担当者名を入力して追加を要求したとき、THE Project_Service SHALL その名前を Project の Members リストに追加する
3. WHEN ユーザーが担当者を削除したとき、THE Project_Service SHALL その名前を Members リストから削除する
4. THE System SHALL 担当者管理はサブプロジェクト一覧画面・タスク一覧画面の両方から操作でき、常に同一の親プロジェクトの Members リストを参照・更新する

---

### 要件 4: サブプロジェクト一覧表示・管理

**ユーザーストーリー:** チームメンバーとして、プロジェクト内のサブプロジェクトを一覧で確認・管理したい。そうすることで、作業の中間階層を整理できる。

#### 受け入れ基準

1. WHEN ユーザーがサブプロジェクト一覧画面を表示したとき、THE System SHALL そのプロジェクトに属するすべての SubProject を一覧表示する
2. WHEN SubProject が1件も存在しないとき、THE System SHALL 「サブプロジェクトがありません」というメッセージを表示する
3. WHEN ユーザーがサブプロジェクトの「タスク一覧」ボタンをクリックしたとき、THE System SHALL そのタスク一覧画面へ遷移する
4. WHEN ユーザーがサブプロジェクト名を入力して作成を要求したとき、THE Project_Service SHALL 新しい SubProject を作成する
5. WHEN ユーザーが SubProject の削除ボタンを押したとき、THE System SHALL 「delete」の入力を求める確認モーダルを表示する
6. WHEN ユーザーが「delete」と入力して削除を実行したとき、THE Project_Service SHALL SubProject とその配下のすべての Task を削除する

---

### 要件 5: タスク一覧・フィルタリング表示

**ユーザーストーリー:** チームメンバーとして、サブプロジェクト内のタスクを一覧で確認・絞り込みたい。そうすることで、進捗状況を把握できる。

#### 受け入れ基準

1. WHEN ユーザーがタスク一覧画面を表示したとき、THE System SHALL そのサブプロジェクトに属するすべての Task を一覧表示する
2. THE System SHALL 各 Task のタイトル・Assignee_Name・Due_Date・Status を一覧上に表示する
3. WHEN Task が1件も存在しないとき、THE System SHALL 「タスクがありません」というメッセージを表示する
4. WHEN ユーザーが Status を指定してフィルタリングを要求したとき、THE System SHALL 指定した Status に一致する Task のみを表示する
5. WHEN ユーザーが Assignee_Name をプルダウンで指定してフィルタリングを要求したとき、THE System SHALL 指定した Assignee_Name に一致する Task のみを表示する
6. WHEN ユーザーが期日の範囲を指定してフィルタリングを要求したとき、THE System SHALL 指定した範囲内の Due_Date を持つ Task のみを表示する
7. WHEN フィルタリングの結果が0件のとき、THE System SHALL 「該当するタスクはありません」というメッセージを表示する
8. WHEN ユーザーが一覧上の Task をクリックしたとき、THE System SHALL そのタスク詳細画面へ遷移する

---

### 要件 6: タスク作成・編集

**ユーザーストーリー:** チームメンバーとして、タスクを作成・編集したい。そうすることで、「誰が・何を・いつまでに」実施するかをチーム全体で共有できる。

#### 受け入れ基準

1. WHEN ユーザーがタイトルを入力してタスク作成を要求したとき、THE Task_Service SHALL 新しい Task を Status「未着手」で作成する
2. THE Task_Service SHALL タスクタイトルを 1文字以上 255文字以下で受け付ける
3. IF タイトルが空文字列のとき、THEN THE Task_Service SHALL バリデーションエラーを返す
4. WHEN ユーザーが Task のタイトル・Assignee_Name・Due_Date・説明・メモを更新したとき、THE Task_Service SHALL 変更内容を保存し、更新後の Task 情報を返す
5. THE Task_Service SHALL Assignee_Name を Project の Members リストからプルダウンで選択する形式で受け付ける
6. WHEN ユーザーが Task を削除したとき、THE Task_Service SHALL Task を削除する
7. WHEN Task の削除が完了したとき、THE System SHALL タスク一覧画面へ遷移する

---

### 要件 7: タスクステータス管理

**ユーザーストーリー:** チームメンバーとして、タスクの進捗状態を更新したい。そうすることで、チーム全体が作業の進み具合を把握できる。

#### 受け入れ基準

1. THE Task_Service SHALL Task の Status として「未着手」「進行中」「完了」の3種類を管理する
2. WHEN ユーザーが Task の Status を更新したとき、THE Task_Service SHALL 変更を保存し更新日時を記録する
3. WHILE Task の Due_Date が現在日時より過去であり Status が「完了」でないとき、THE System SHALL その Task を期限超過状態として視覚的に区別して表示する

---

### 要件 8: タスク詳細表示・インライン編集

**ユーザーストーリー:** チームメンバーとして、タスクの詳細情報を確認・編集したい。そうすることで、タスクの内容・担当者・期限・ステータスを素早く更新できる。

#### 受け入れ基準

1. WHEN ユーザーがタスク詳細画面を表示したとき、THE System SHALL Task のタイトル・Assignee_Name・Due_Date・Status・説明・メモ・関連資料をすべて表示する
2. THE System SHALL タイトル・担当者・期限・説明の各項目に「変更」ボタンを提供し、クリックするとインライン編集できる
3. THE System SHALL メモ欄はクリックまたは「編集」ボタンでインライン編集できる
4. THE System SHALL 関連資料欄は「変更」ボタンを押すと追加・編集・削除ができる
5. THE System SHALL メモ・関連資料は「詳細を表示」ボタンを押すと展開される折りたたみ形式で表示する
6. WHEN ユーザーがタスク詳細画面から戻るボタンを押したとき、THE System SHALL タスク一覧画面へ遷移する

---

### 要件 9: 担当者別タスク一覧

**ユーザーストーリー:** チームメンバーとして、特定の担当者が抱えているすべてのタスクを横断的に確認したい。そうすることで、誰がどのような作業を持っているかを一目で把握できる。

#### 受け入れ基準

1. WHEN ユーザーが担当者別タスク画面を表示したとき、THE System SHALL タスクに割り当てられているすべての担当者名を一覧表示する
2. WHEN ユーザーが担当者名を選択したとき、THE System SHALL その担当者に割り当てられたすべての Task をプロジェクト別にグループ化して表示する
3. THE System SHALL 担当者別タスク一覧において期限超過タスクを視覚的に区別して表示する
4. THE System SHALL 担当者別タスク一覧において未完了タスク件数と全タスク件数を表示する
5. WHEN ユーザーがタスクをクリックしたとき、THE System SHALL そのタスク詳細画面へ遷移する

---

### 要件 10: パンくずナビゲーション

**ユーザーストーリー:** チームメンバーとして、現在の階層位置を把握し、上位階層へ素早く移動したい。

#### 受け入れ基準

1. THE System SHALL サブプロジェクト一覧・タスク一覧・タスク詳細の各画面において、「プロジェクト一覧 › プロジェクト名 › サブプロジェクト名 › タスク名」の形式でパンくずリストを表示する
2. WHEN ユーザーがパンくずリストの上位階層をクリックしたとき、THE System SHALL 該当の画面へ直接遷移する
