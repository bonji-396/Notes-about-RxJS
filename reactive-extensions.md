# ReactiveX (Reactive Extensions)

ReactiveX（Reactive Extensions）は、リアクティブプログラミングをサポートするためのライブラリの一つで、様々なプログラミング言語で利用できるように設計されています。ReactiveXは、オブザーバーパターン、イテレーターパターン、および関数型プログラミングを組み合わせて、非同期データフローの管理を簡単にすることを目的としています。
これはリアクティブプログラミングの実装であり、各プログラミング言語で実装されるツールの概念を提供します。

- Website: https://reactivex.io

## ReactiveXの主な特徴

- 多言語対応: RxJava（Java用）、RxJS（JavaScript用）、RxSwift（Swift用）、Rx.NET（.NET用）など、多くのプログラミング言語に対応しています。
- 非同期データストリームの操作: ReactiveXは、データストリームを作成、変換、組み合わせ、および消費するための豊富なオペレーターを提供します。
- エラー処理: エラーはデータストリームの一部として扱われ、エラーハンドリングが容易になります。
- バックプレッシャーのサポート: プロデューサーとコンシューマー間のデータフローの速度を調整するための機能を提供します。

## ReactiveXとリアクティブプログラミングの関係

ReactiveXは、リアクティブプログラミングの概念を具体的に実装するツールの一つです。リアクティブプログラミングの基本原則に従い、データフローやイベント駆動のアプリケーションを簡単に構築することができます。ReactiveXは特に、複雑なデータフローとイベント処理のロジックを、簡潔かつ宣言的に記述する能力をプログラマーに提供します。

例えば、RxJSを使用するWebアプリケーションでは、ユーザー入力、APIレスポンス、またはWebSocket通信からのイベントをリアクティブなストリームとして扱い、これらのストリームを効果的に管理し、UIをリアルタイムに更新することができます。

総じて、ReactiveXはリアクティブプログラミングのアプローチを促進し、拡張するための強力なライブラリであり、非同期的でイベント駆動のアプリケーション開発において大きな役割を果たしています。

## ReactiveXの主要要素

ReactiveXライブラリにおける次の各要素は、リアクティブプログラミングにおいてデータの流れを制御し、複雑な非同期処理を簡素化する役割を果たします。

### Observable

Observableはデータまたはイベントのシーケンスを非同期に生成し、それを購読するオブザーバーに配信します。Observableはデータの発生源であり、0個以上のデータアイテムを配信し、正常に完了するか、エラーにより終了することができます。購読することでデータを受信し、データが到着するたびに指定した処理を行うことができます。

### Observer
Observerは、Observableからデータストリームを監視し、データが到達するとそれを受信して処理を行います。Observableが生成するデータをリアルタイムで処理し、受け取るデータに応じてフロー制御を行うことができます。ObserverはObservableに「購読」されることによって、データの流れに接続されます。

### Operators

OperatorsはObservableが生成するデータストリームを操作するためのメソッドです。これには、マッピング、フィルタリング、変換、結合など多岐にわたる操作があります。これらのオペレーターを使用することで、複雑なデータフローを簡単に構築し、デバッグが容易な、宣言的なプログラミングスタイルを実現できます。

### Single

Singleは、Observableと似ていますが、単一のデータ値またはエラーを発行することに特化しています。Singleは、ちょうど1つのデータを生成するような一回限りの操作（例えば、HTTPリクエストによるレスポンスの取得など）に適しています。Singleは、成功時にデータを発行し、その後に完了するか、エラーを発行します。

### Subject

SubjectはObservableとオブザーバーの両方の特性を持つ、特殊なタイプのObservableです。外部からイベントをプッシュすることができ、同時にそれを購読している複数のオブザーバーにイベントをマルチキャスト（一つのイベントソースから複数のリスナーへの配信）することができます。Subjectは、状態を保持する必要がある場合や、イベントを動的に発行する場合に便利です。

### Scheduler

SchedulerはObservableがオペレーション（データの発行や変換）を実行するタイミングと場所（スレッドやタスクキューなど）を管理します。これにより、例えばメインスレッドでデータを生成してバックグラウンドスレッドで処理するなど、非同期処理の精密な制御が可能になります。Schedulerはリアクティブプログラミングにおいて、パフォーマンスの最適化やリソースの効率的な使用を実現するための重要なツールです。

これらのコンポーネントを組み合わせることで、ReactiveXを使用する開発者は複雑なデータフローとイベント駆動のロジックを効率的に扱うことができます。

## Observable
ObservableはReactiveXにおいて核となる概念であり、データまたはイベントのシーケンスを表すオブジェクトです。Observableは時間とともにゼロ個以上のデータアイテムを非同期に発行し、これらのデータをObserverにプッシュすることで、リアクティブなプログラミングモデルを実現します。

### Observableの主要な特徴

1. データの非同期発行:  
Observableは非同期にデータを生成し、これによりメインスレッドのブロックを防ぎながら、複数のデータソースからのデータを効率的に扱うことができます。
2. マルチキャスティング:  
Observableは、複数のObserverに対してデータを同時に配信することができます。これにより、同一のデータストリームを異なる処理や目的で利用することが可能です。
3. コンポーズ可能:  
Observableは、様々なオペレーターを通じて他のObservableと組み合わせたり、変換したりすることができます。これにより、複雑なデータ変換やフィルタリング、集約などの操作が容易になります。
4. エラー処理:  
Observableはエラーをデータとしてストリーム内に含むことができるため、エラーが発生した場合の処理をObserverが適切にハンドルできます。
5. 資源のクリーンアップ:  
Observableがデータの発行を完了した後、購読を終了するためのonCompletedメソッドが呼び出され、これを通じて開放すべきリソースのクリーンアップが行われます。

### Observableの動作の流れ

1. 購読の開始:  
ObserverがObservableに購読することで、データの配信が開始されます。
2. データの発行:  
ObservableはonNextメソッドを通じてObserverにデータを配信します。これは何度も繰り返されることがあります。
3. エラーまたは完了の通知:  
エラーが発生した場合、ObservableはonErrorを呼び出してエラーを通知します。すべてのデータが正常に発行された場合は、onCompletedが呼び出され、データの配信が終了します。

Observableはリアクティブプログラミングにおいてデータやイベントの流れを定義し、アプリケーションの動的な振る舞いを設計するための基盤を提供します。これにより、開発者は効率的なデータ処理とリアルタイムのアプリケーションレスポンスを実現することができます。

## Observer
ReactiveXにおけるObserverは、リアクティブプログラミングにおいて中心的な役割を果たす要素の一つです。Observerは、Observableからデータストリームを監視し、データが到達するとそれを受信して処理を行います。ObserverはObservableに「購読」されることによって、データの流れに接続されます。

### Observerの主要なメソッド

1. onNext:  
Observableから新しいデータアイテムが配信されるたびに呼び出されます。このメソッドは、データストリームの各アイテムに対して実行される操作を定義します。
2. onError:  
Observableがエラーを遭遇した場合に呼び出されます。このメソッドはエラーを処理するロジックを含み、通常、エラーメッセージのログ出力やユーザーへの通知などが行われます。
3. onCompleted:  
Observableがデータの発行を完了したとき（すなわち、これ以上送信するデータがないとき）に呼び出されます。このメソッドは、購読の終了処理を行うために使用されることが多く、リソースのクリーンアップなどが行われます。

### Observerの役割

-	データ処理:  
Observerは、Observableが生成するデータをリアルタイムで処理します。これにより、動的なアプリケーション状態の更新やユーザーインターフェースの即時反映などが可能になります。
-	フロー制御:  
Observerは、受け取るデータに応じてフロー制御を行うことができます。例えば、特定の条件下でのみデータを処理する、データが一定数に達したら処理を停止するなどの制御が可能です。
-	エラー管理:  
エラーが発生した場合、Observerはこれを捕捉し適切に対応します。プログラムの安定性と堅牢性を保つために重要な役割を果たします。
-	リソース管理:  
購読が完了した後、Observerは必要なクリーンアップを実行することができます。これにより、メモリリークや不要なプロセスの実行を防ぐことができます。

Observerのこれらの機能は、リアクティブプログラミングにおいて非常に重要であり、データ駆動型のアプリケーション開発において効果的なデータの管理と処理を実現します。

## Operators

ReactiveXにおけるOperatorsは、データストリームを操作するための強力なツールです。これらはObservableのインスタンスを変換、組み合わせ、フィルターするなどの処理を行う関数です。Operatorsを使用することで、複雑なデータ変換や条件に基づいたデータの操作が可能になります。

Operatorsは、リアクティブプログラミングにおいてデータフローを柔軟に制御し、複雑なビジネスロジックを効率的に実装するための重要な要素です。それぞれのオペレーターは特定の機能を持ち、これを組み合わせることで高度なデータ処理が可能になります。

### Operators種類

#### 観測可能なものを作成する
新しいオブザーバブル(データストリーム)を生み出すオペレーター。
- Create:  
オブザーバーメソッドをプログラムで呼び出すことで、オブザーブルをゼロから作成します。
- Defer:  
オブザーバーが購読するまでオブザーバーブルを作成しず、オブザーバーごとに新しいオブザーバーブルを作成します。
Empty/Never/Throw :  
create Observables that have very precise and limited behavior
- From:  
他のオブジェクトまたはデータ構造をオブザーブ可能に変換する
- Interval  
特定の時間間隔で間隔を空けて整数のシーケンスを放出するObservableを作成します。
- Just:  
オブジェクトまたはオブジェクトのセットを、それまたはそれらのオブジェクトを放出するオブザーバブルに変換します。
- ange:  
一連の連続整数を放出するオブザーブブルを作成する
- Repeat:  
特定のアイテムまたは一連のアイテムを繰り返し発行するオブザーブブを作成します。
- Start:  
関数の戻り値を発行するObservableを作成します。
- Timer:  
特定の遅延後に単一のアイテムを放出するオブザーブブを作成する


#### 観測可能な変換

オブザーブブルによって放出されたストリーム内のアイテムを変換する演算子。

- Buffer:  
オブザーブ可能なアイテムを定期的にバンドルに集め、アイテムを一度に1つずつ放出するのではなく、これらのバンドルを放出します。
- FlatMap:  
オブザーバブルから放出されたアイテムをオブザーバブルに変換し、それらからの放出を単一のオブザーバブルに平らにします。
- GroupBy:  
オブザーバブルをオブザーバブルのセットに分割し、それぞれが元のオブザーバブルから異なるアイテムグループを放出し、キーごとに整理されます。
- Map:  
各アイテムに関数を適用することにより、オブザーブブルによって放出されたアイテムを変換します。
- Scan:  
オブザーブブルによって放出された各アイテムに関数を順番に適用し、各連続した値を放出します。
- Window:  
定期的にオブザーブ可能なウィンドウにアイテムをサブサブディバブルから、一度に1つずつアイテムを放出するのではなく、これらのウィンドウを放出します。

#### 観測可能なフィルタリング

観測可能なソースから項目を選択的に放出する演算子。条件に基づいてデータをフィルタリングします。

- Debounce:  
特定の期間が経過し、別のアイテムを放出せずにオブザーブできるアイテムのみをオブザーブできます。
- Distinct:  
オブザーブ可能なものによって放出された重複したアイテムを抑制する
- ElementAt:  
観測可能によって放出された項目nのみを放出する
- Filter:  
述語テストに合格したオブザーブからそれらの項目のみを放出する
- First:  
オブザーブから最初のアイテム、または条件を満たす最初のアイテムのみを放出する
- IgnoreElements:  
オブザーブからアイテムを発行せず、終了通知をミラーリングします。
- Last:  
オブザーブ可能によって放出された最後の項目のみを放出する
- Sample:  
定期的な時間間隔内にオブザーブから放出された最新のアイテムを放出する
- Skip:  
オブザーブブルによって放出された最初のn個の項目を抑制する
- SkipLast:  
オブザーブ可能によって放出された最後のn個の項目を抑制する
- Take:  
オブザーブ可能によって放出された最初のn項目のみを放出する
- TakeLast:  
オブザーブ対象によって放出された最後のn個の項目のみを放出する

#### 観測可能な組み合わせ
複数のソースオブザーバブルを使用して単一のオブザーバブルを作成する演算子。複数のストリームを一つに結合します。

- And/Then/When :  
combine sets of items emitted by two or more Observables by means of Pattern and Plan intermediaries
- CombineLatest:  
アイテムが2つのオブザーバブルのいずれかによって放出された場合、指定された関数を介して各オブザーバブルによって放出された最新のアイテムを組み合わせて、この関数の結果に基づいてアイテムを放出します。
- Join:  
他のオブザーバブルから放出されたアイテムに従って定義された時間ウィンドウ中に1つのオブザーバブルからのアイテムが放出されるたびに、2つのオブザーバブルから放出されたアイテムを組み合わせる
- Merge:  
複数の観測可能なものを1つに結合し、それらの排出量をマージする
- StartWith:  
ソースオブザーブからアイテムを発行する前に、指定された一連のアイテムを発行します。
- Switch:  
オブザーバブルを放出するオブザーバブルを、それらのオブザーバブルの中で最も最近放出されたアイテムを放出する単一のオブザーバブルに変換する
- Zip:  
指定された関数を介して複数の観測物の放出を結合し、この関数の結果に基づいて組み合わせごとに単一の項目を放出します。


#### エラー処理オペレーター
オブザーブ可能なエラー通知からの回復を支援するオペレーター。エラーが発生したときの挙動を定義します。

- Catch:  
エラーなしでシーケンスを継続することで、onError通知から回復します。
- Retry:  
ソースObservableがonError通知を送信した場合、エラーなしで完了することを期待して再購読してください。

#### 観測可能なユーティリティオペレーター
オブザーバブルを扱うための便利なオペレーターのツールボックス。ログ出力、デバッグ情報の提供など、補助的な操作を行います。

- Delay:  
排出量を観測可能から時間内に特定の量でシフトする
- Do:  
さまざまなオブザーブ可能なライフサイクルイベントに対して実行するアクションを登録する
- Materialize/Dematerialize :  
発行されたアイテムと送信されたアイテムとして送信された通知の両方を表すか、このプロセスを逆にします。
- ObserveOn:  
オブザーバーがこのオブザーブルをオブザーブするスケジューラを指定する
- Serialize:  
オブザーブルにシリアル化された通話を行い、適切に振る舞うように強制する
- Subscribe:  
観測可能物質からの排出と通知に基づいて操作する
- SubscribeOn:  
オブザーブルが購読時に使用すべきスケジューラを指定します
- TimeInterval:  
アイテムを放出するオブザーバーブルを、それらの放出の間に経過した時間の量を示すものを放出するものに変換します。
- Timeout:  
ソースの観測可能をミラーリングしますが、特定の期間が経過しても、放出されたアイテムがない場合、エラー通知を発行します。
- Timestamp:  
オブザーブブルによって放出された各アイテムにタイムスタンプを付ける
- Using:  
オブザーブ可能なものと同じ寿命を持つ使い捨てリソースを作成する

#### 条件付きおよびブール演算子
1つ以上の観測可能物または観測可能物によって放出された項目を評価する演算子。条件に基づいてストリームを操作します

- All:  
観測可能なものによって放出されたすべての項目がいくつかの基準を満たしているかどうかを判断する
- Amb:  
2つ以上のソースオブザーバブルが与えられ、これらのオブザーバブルの最初のものからすべてのアイテムを放出してアイテムを放出します。
- Contains:  
オブザーブブルが特定のアイテムを放出するかどうかを判断する
- DefaultIfEmpty:  
ソースオブザーバブルからアイテムを発行するか、ソースオブザーバブルが何も発行しない場合はデフォルトアイテムを発行します。
- SequenceEqual:  
2つのオブザーバブルが同じ一連の項目を放出するかどうかを判断する
- SkipUntil:  
2番目のオブザーバブルがアイテムを放出するまで、オブザーブから放出されたアイテムを破棄する
- SkipWhile:  
指定された条件がfalseになるまで、Observableによって放出されたアイテムを破棄する
- TakeUntil:  
2番目のオブザーバブルがアイテムを放出または終了した後、オブザーブから放出されたアイテムを破棄する
- TakeWhile:  
指定された条件が偽になった後、Observableによって放出されたアイテムを破棄する

#### 数学演算子と集計演算子

オブザーブ可能によって放出されたアイテムのシーケンス全体を操作する演算子。ストリームのデータに対して集約や数学的操作を行います。

- Average:  
観測可能によって放出された数値の平均を計算し、この平均を放出します
- Concat:  
2つ以上の観測可能物から放出する排出物をインターリーブせずに排出する
- Count:  
ソースオブザーブから放出されたアイテムの数を数え、この値のみを放出します。
- Max:  
オブザーブルによって放出された最大値の項目を決定し、放出する
- Min:  
オブザーブルによって放出された最小値の項目を決定し、放出する
- Reduce:  
観測可能によって放出された各項目に順次関数を適用し、最終値を放出する
- Sum:  
観測可能によって放出された数字の合計を計算し、この合計を放出する

#### 背圧オペレーター
- backpressure - 観測者が消費するよりも早くアイテムを生産する観測物に対処するための戦略。データのフローを制御します。


#### 接続可能な観測可能な演算子
より正確に制御されたサブスクリプションダイナミクスを持つスペシャリティオブザーバーブル

- Connect:  
コネクティブルオブザーブルに、加入者にアイテムの送信を開始するように指示する
- Publish:  
通常のオブザーブを接続可能なオブザーブに変換する
- RefCount:  
コネクチャブルオブザーブブルを通常のオブザーブブルのように動作させる
- Replay:  
オブザーバーがアイテムの放出を開始した後に購読した場合でも、すべてのオブザーバーが同じ一連のアイテムを見るようにする

#### 観測可能を変換するオペレーター
- To:  
オブザーブを別のオブジェクトまたはデータ構造に変換する


## Single
ReactiveXにおけるSingleは、Observableの特殊な形態であり、一つのデータイベントまたはエラーイベントのみを発行することに特化しています。これは、厳密に一つのデータ項目を生成するような操作に対して適しており、その挙動は以下のように特徴づけられます。

### Singleの主要な特徴

1. データまたはエラーの単一発行:  
Singleは正確に一つのデータを発行するか、あるいはエラーを発行します。これにより、Singleは0個または複数個のデータを発行する可能性があるObservableとは異なり、より単純な非同期処理に適しています。
2. シンプルなAPI:  
Singleは、データやエラーを発行した後に完了するため、ObservableよりもAPIがシンプルです。このため、扱うべきシナリオが限定され、その分理解しやすくなります。
3. ユースケースの特定:  
SingleはHTTPリクエストのような一回限りの応答を扱う場面や、一つの結果を返すデータベースクエリなど、一つの結果を期待する操作に最適です。

### Singleの動作の流れ

1. 購読の開始:  
Observerまたは専用のSingleObserverがSingleに購読します。
2. データの発行:  
SingleはonSuccessを呼び出し、購読者にデータを提供します。この時点でデータフローは完了します。
3. エラーの通知:  
もし何らかのエラーが発生した場合、SingleはonErrorを呼び出してエラーを通知します。この後、データフローは終了します。

### SingleとObservableの違い

-	発行するデータの量:  
Observableは0個以上のデータを任意のタイミングで発行できますが、Singleは一つのデータまたはエラーのみを発行します。
-	用途の違い:  
Observableは連続したデータストリームを扱うのに対し、Singleは一つの結果を扱う非同期処理に特化しています。

Singleは、そのシンプルさからデータフローの管理を容易にし、一つの値を非同期に取得するような場合に適しているため、API応答の処理など特定の用途において非常に有効です。

### RxJS では通常の Observable を使ってSingleと同じ振る舞いを実現することが一般的
RxJS では Single クラスが特別に存在するわけではありませんが、Observable を使って一つの値のみを発行して完了するようなストリームを作成することができます。

RxJS で一つのデータ値を扱う場合、first(), single(), あるいは take(1) などのオペレーターを使用することで、Observable から一つのデータを取得して処理を完了させることができます。これにより、Single が持つ特性をエミュレートすることが可能です。

例

```js
import { of } from 'rxjs';
import { first } from 'rxjs/operators';

const observable = of(1, 2, 3);

const singleValue = observable.pipe(first());
singleValue.subscribe({
  next: value => console.log(value),
  complete: () => console.log('Completed')
});

// 出力:
// 1
// Completed
```
このコードでは、of(1, 2, 3) で生成される Observable から first() オペレーターを使用して最初の一つの値のみを取得し、その後完了します。これにより、RxJS でも Single のように一つの値のみを扱うストリームを簡単に作成することができます。

## Subject
ReactiveXライブラリにおけるSubjectは、とても強力で多用途なコンポーネントです。Subjectは同時にObservableとObserverの両方の特性を持ち、データを受け取り（Observerとして）、購読者（Observers）にそのデータをマルチキャストする能力があります（Observableとして）。この特性により、Subjectはデータのソースとしても、データのブロードキャスターとしても機能することができます。

### Subjectの主要な特性と用途

1. マルチキャスト:  
通常のObservableは各購読者に対して独立したデータストリームを生成しますが、Subjectは同じデータイベントを複数の購読者に同時に配信することができます。これにより、データの共有やイベントのブロードキャスティングが効率的に行えます。
2. データの収集と再発行:  
Subjectは受け取ったデータを内部に保持することなく、直ちにそれを購読者に向けて発行します。これにより、イベント駆動型のシナリオやリアルタイムデータフィードに適しています。
3. ステートフル操作:  
特定の種類のSubject（例えばBehaviorSubjectやReplaySubject）は、過去のある数のデータを保持し、新たな購読者に対してそのデータを再発行することができます。これにより、状態の同期や遅延した購読のサポートが可能になります。

### Subjectの主要な種類

1. BehaviorSubject:  
最後に発行されたアイテムを記憶し、新しい購読者が購読を開始したときにそのアイテムを発行します。初期値を持つため、購読時に必ずデータが得られることが保証されています。
2. ReplaySubject:  
発行されたデータを指定した数または時間にわたって記憶し、新規購読者に対してそのデータを再発行します。これにより、過去のデータストリームに後からアクセスすることが可能です。
3. AsyncSubject:  
Observableが完了すると（onComplete()が呼ばれた後）、最後に発行されたアイテムのみを購読者に発行します。データの最終状態のみを配信する際に便利です。

### 使用例
```js
import { Subject } from 'rxjs';

const subject = new Subject<number>();

subject.subscribe({
  next: (v) => console.log(`observerA: ${v}`)
});
subject.subscribe({
  next: (v) => console.log(`observerB: ${v}`)
});

subject.next(1);
subject.next(2);
```
この例では、Subjectが2つの購読者に対してデータ1と2を発行しています。これにより、それぞれの購読者は同じデータを受け取ることができます。

Subjectはその柔軟性と強力な機能により、イベントバス、ステート管理、データフィードの共有など、多くのリアクティブプログラミングのシナリオで重要な役割を果たします。

## Scheduler
ReactiveXのSchedulerは、オブザーバブルの実行をコントロールするコンポーネントで、オペレーション（データの発行や変換など）がどのようなコンテキスト（例えば、どのスレッドやタスクキュー）で実行されるかを指定します。Schedulerは非同期プログラミングの複雑さを抽象化し、異なるスレッド間で作業を簡単に移動することができるように設計されています。

### Schedulerの主な役割

1. スレッド管理:  
Schedulerは特定のスレッドやスレッドプールでオブザーバブルのオペレーションを実行することができます。これにより、UIスレッドをブロックすることなくバックグラウンドでデータ処理を行ったり、UIスレッドで安全にUI更新を行うことが可能です。
2. 実行のスケジューリング:  
オペレーションの実行タイミングを精密にコントロールし、例えば遅延実行や定期実行をスケジュールすることができます。
3. リソースの最適化:  
適切なSchedulerを使用することで、アプリケーションのパフォーマンスを向上させ、リソースの使用を最適化することができます。

### RxJSでのSchedulerの主な種類

1. asyncScheduler:  
非同期操作に適した、setTimeoutやsetIntervalを使ったスケジューラーです。
2. asapScheduler:  
マイクロタスクキューを利用して、現在のイベントサイクルが完了するとすぐに実行されるスケジューラーです。
3. queueScheduler:  
同期的な実行で、タスクをキューに追加し、FIFO（先入れ先出し）順に実行します。
4. animationFrameScheduler:  
requestAnimationFrameを使って、ブラウザのアニメーションフレームでタスクを実行するスケジューラーです。

### 使用例（RxJS）
```js
import { of, asyncScheduler } from 'rxjs';
import { observeOn } from 'rxjs/operators';

const observable = of(1, 2, 3).pipe(
  observeOn(asyncScheduler)
);

observable.subscribe({
  next: val => console.log(`Received: ${val}`)
});
```
この例では、asyncSchedulerを使用して、ofによって生成された値の消費（購読）を非同期に行っています。これにより、例えばUIスレッドをブロックせずに処理を進めることができます。

Schedulerの適切な使用は、特に複数のスレッドを使用するアプリケーションや、リソースの利用が重要な場面でのアプリケーションのパフォーマンスと応答性を大きく向上させることができます。