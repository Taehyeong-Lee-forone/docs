# System Architecture (MVP)

MVPアーキテクチャ図

```mermaid
flowchart LR
    subgraph Client[iOS App (FlutterFlow)]
      UI[UI層\nホーム/体重/筋トレ/食事/FAB]
      Cache[ローカルキャッシュ/ローカル通知]
      UI --> Cache
    end

    subgraph Firebase[Firebase / GCP]
      Auth[Firebase Auth]
      FS[(Cloud Firestore)]
      Storage[(Cloud Storage)]
      Func[Cloud Functions]
      Analytics[Analytics / Crashlytics]
      Logging[Cloud Logging]
    end

    User[選手ユーザー] --> Client
    Client --> Auth
    Client --> FS
    Client --> Storage
