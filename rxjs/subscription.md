# Subscription
Subscription は RxJS でのリアクティブプログラミングにおける購読管理の中心となる重要な概念です。適切に管理することで、アプリケーションのパフォーマンスを向上させ、リソースリークを防ぐことができます。

Subscription は、Observable からデータの流れ（ストリーム）を消費するためのオブジェクトです。Subscription オブジェクトは、Observable が発行するデータを受け取ると同時に、そのデータストリーム（Observable）を購読の管理と制御を行います。

1. データの受信  
Observable が発行するデータを受け取り、それに応じて指定された動作（関数）を実行します。これには、データの加工、表示、保存などが含まれます。
2. リソースの解放  
Subscription は、不要になった資源を適切に解放するためのメカニズムを提供します。これにより、メモリリークを防ぐことができます。
3. 購読の制御  
購読を開始した後、任意のタイミングでこれを停止（unsubscribe）することができます。これにより、Observable からのデータ発行が停止され、リソースの解放が行われます。

Subscriptionは、使い捨てリソースを表すオブジェクトであり、通常はオブザーブブルの実行です。

## unsubscribe()
Subscriptionには、リソースを解放したり、Observableの実行をキャンセルしたりするためのunsubscribe()メソッドがあります。


```js
import { interval } from 'rxjs';

const observable = interval(1000);
const subscription = observable.subscribe(x => console.log(x));

// これは、Observer で subscribe を呼び出すことによって開始された
// 進行中の Observable 実行をキャンセルします。
setInterval(() => {
  subscription.unsubscribe();
}, 5000);

// 出力:
// 0
// 1
// 2
// 3
// 4
```
unsubscribe()メソッドを呼び出すことで、Observable の購読を終了し、内部的に確保されているリソースを解放します。これは特に、無限のデータストリーム（例：interval など）を扱う場合や、コンポーネントが破棄されるタイミング（例：Webアプリのページ遷移）で重要になります。

```js
import { from } from 'rxjs';

const observable = from([1, 2, 3, 4, 5]);

const subscription = observable.subscribe({
  next(x) { console.log('Got value ' + x); },
  error(err) { console.error('Something wrong occurred: ' + err); },
  complete() { console.log('Done'); }
});

// データの受信が終わった後、または必要ない場合に購読を解除
subscription.unsubscribe();
```



## Subscription の連鎖
サブスクリプションをまとめることもできます。そ
複数の Subscription を管理する場合、それらを一つの Subscription オブジェクトにまとめ、一括で unsubscribe することもできます。

```js
import { interval } from 'rxjs';

const observable1 = interval(400);
const observable2 = interval(300);

const subscription = observable1.subscribe(x => console.log('first: ' + x));
const childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  // subscriptionとchildSubscriptionの両方を解除します
  subscription.unsubscribe();
}, 1000);

// 出力:
// second: 0
// first: 0
// second: 1
// first: 1
// second: 2
```