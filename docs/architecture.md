```mermaid
flowchart LR
    %% ====== クライアント ======
    subgraph Client["iOSアプリ - FlutterFlow"]
        UI["UI: ホーム / 体重 / 筋トレ / 食事 / FAB"]
        Cache["ローカルキャッシュ & ローカル通知"]
        UI --> Cache
    end

    %% ====== Firebase / GCP ======
    subgraph Firebase["Firebase / GCP"]
        Auth["Firebase認証 - Apple / メール"]
        FS["Cloud Firestore: users, weights, workouts, workout_sets, meals, daily_notes"]
        Storage["Cloud Storage - 食事写真"]
        Func["Cloud Functions: バリデーション, 集計, 通知(将来)"]
        Analytics["Analytics / Crashlytics"]
        Logging["Cloud Logging / Error Reporting"]
    end

    %% ====== 運用・セキュリティ ======
    subgraph Ops["運用・セキュリティ"]
        Rules["セキュリティルール: Firestore / Storage"]
        AppCheck["App Check"]
        Envs["環境: 開発 / 本番"]
        Sched["Cloud Scheduler -> FCM (将来)"]
    end

    %% ====== ユーザーとフロー ======
    User["選手ユーザー"] -->|アプリ利用| Client
    Client -->|サインイン| Auth
    Client -->|CRUD: 体重 / 筋トレ / 食事 / メモ| FS
    Client -->|写真アップロード| Storage
    Client -->|イベント送信| Analytics
    Func -->|集計・検証| FS

    %% ====== 監視・ログ ======
    Auth --> Logging
    FS --> Logging
    Storage --> Logging
    Func --> Logging

    %% ====== 保護 ======
    Rules -.保護.-> FS
    Rules -.保護.-> Storage
    AppCheck -.トークン検証.-> Client
    Sched -.トリガー.-> Func
