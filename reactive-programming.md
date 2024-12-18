# リアクティブプログラミング

リアクティブプログラミングは、データフローや変更の伝播を中心に構築されるプログラミングパラダイムです。
これは、アプリケーションがデータの変更を効率的に処理し、ユーザーインタフェースが動的に更新されるように設計されています。

イベント駆動型プログラミングの一種であり、特に非同期なデータストリームの扱いに焦点を当てています。
データの流れ（ストリーム）を中心に考え、その流れに対して反応（リアクション）する方式でプログラムを構築します。

## 基本的な概念
以下は、リアクティブプログラミングの主な特徴と概念です。

### データストリーム（Streams）
リアクティブプログラミングでは、データは時間に沿って進化する一連のイベントとして表されます。これをデータストリームと呼びます。
イベント、ユーザ入力、ファイルの読み込み、HTTPリクエストなどの形で表現され、例えば、ユーザーのクリックイベント、APIからのレスポンス、変数の値の変更などがストリームになりえます。

### イベント駆動（Event-driven）

アプリケーションはイベントに基づいて反応し、イベントが発生するとデータストリームが更新されます。これにより、イベントの発生に応じて動的なレスポンスが可能になります。

### 非同期処理（Asynchronous）

リアクティブプログラミングは非同期処理を中心に構築されています。これによりロッキング操作が少なく、システムが複数のタスクを効率的に同時に処理できるようになります。

### オブザーバー（Observer）

オブザーバーパターンはリアクティブプログラミングにおいて中心的な役割を果たします。
オブザーバー（観察者）はサブジェクト（観察対象: データの提供者）を監視し、サブジェクトの状態に変化があると通知を受け取り、適切な処理を行います。これは、ストリームのデータが変更されたときに自動的に更新を行うUIコンポーネントによく見られるパターンです。

### バックプレッシャー（Backpressure）

ストリームが速すぎて処理しきれない場合、リアクティブシステムは「バックプレッシャー」と呼ばれる機構を使用してデータの流れを制御します。

例えば、プロデューサー（Producers: データの生成者）がコンシューマー（Consumers: データの消費者）よりも速くデータを生成する場合、システムはバックプレッシャーを使用してデータフローの速度を調整します。これにより、コンシューマーがオーバーロードすることなくデータを処理できます。

## 特徴
以下のリアクティブプログラミングの特徴により、大規模なデータを扱うアプリケーションやリアルタイムの対応が求められるアプリケーションを効果的に開発することを可能にします。

### スケーラビリティ
リアクティブプログラミングは、データの流れを効率的に管理し、システムリソースを有効に活用することで、アプリケーションのスケーラビリティを向上させます。

### レスポンシブ
システムはイベントに迅速に反応し、ユーザーのアクションに対して即座にフィードバックを提供することができます。

### レジリエンス
障害に対して堅牢で、一部のコンポーネントの失敗がシステム全体に影響を及ぼすことなく、継続的に運用できるように設計されています。

### イベント合成
複数のイベントストリームを組み合わせて、新しいストリームを生成することができます。これにより、複雑なデータフローとロジックをシンプルに扱うことが可能になります。


