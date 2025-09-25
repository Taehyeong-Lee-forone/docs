```mermaid
flowchart LR
    %% ====== Client ======
    subgraph Client["iOS App - FlutterFlow"]
        UI["UI: Home / Weight / Workout / Food / FAB"]
        Cache["Local cache & Local notifications"]
        UI --> Cache
    end

    %% ====== Firebase / GCP Core ======
    subgraph Firebase["Firebase / GCP"]
        Auth["Firebase Auth - Apple / Email"]
        FS["Cloud Firestore: users, weights, workouts, workout_sets, meals, daily_notes"]
        Storage["Cloud Storage - meal photos"]
        Func["Cloud Functions: validation, aggregation, notifications (future)"]
        Analytics["Analytics / Crashlytics"]
        Logging["Cloud Logging / Error Reporting"]
    end

    %% ====== Ops / Security ======
    subgraph Ops["Ops & Security"]
        Rules["Security Rules: Firestore / Storage"]
        AppCheck["App Check"]
        Envs["Environments: dev / prod"]
        Sched["Cloud Scheduler -> FCM (future)"]
    end

    %% ====== Actors & Flows ======
    User["Athlete user"] -->|Use app| Client
    Client -->|Sign-In| Auth
    Client -->|CRUD weights / workouts / meals / notes| FS
    Client -->|Upload photo| Storage
    Client -->|Events| Analytics
    Func -->|Aggregate & Validate| FS

    %% ====== Observability ======
    Auth --> Logging
    FS --> Logging
    Storage --> Logging
    Func --> Logging

    %% ====== Protections ======
    Rules -.protects.-> FS
    Rules -.protects.-> Storage
    AppCheck -.verifies tokens.-> Client
    Sched -.triggers.-> Func
