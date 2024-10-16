# Observable

RxJSのObservableは、非同期処理やイベントベースのプログラミングを強力にサポートするコアオブジェクトです。Observableはデータまたはイベントのストリームを表し、時間の経過とともにゼロまたは複数の値を非同期に生成して送出することができます。

## データ受け渡しと、扱うデータの単位

データの受け渡しには「Pull」と「Push」の二つの方式があります。Pull方式では、コンシューマーがアクティブにデータを要求し、プロデューサーはパッシブに要求に応じてデータを提供します。一方、Push方式では、プロデューサーがアクティブにデータを生成し、コンシューマーは受け取ったデータにパッシブに反応します。これらはそれぞれ単一値や複数値を同期的または非同期に扱う方式として、Function、Iterator、Promise、Observableに対応します。

||SINGLE|MULTIPLE|
|---|---|---|
|Pull|Function|Iterator|
|Push|Promise|**Observable**|

- Function: 一つの値を同期的に返す
- Iterator: 複数の値を順に同期的に返す
- Promise: 一つの値を非同期に返す
- Observable: 複数の値を非同期に送出する

つまり、オブザーバブルは、複数の値の遅延プッシュコレクションです。


### Pull
データの消費者が必要に応じてデータを要求する方式です。

プルシステムでは、コンシューマーはデータプロデューサーからいつデータを受け取るかを決定します。プロデューサ自身は、データがいつコンシューマーに配信されるかを知らない。

すべてのJavaScript関数はプルシステムです。関数はデータのプロデューサーであり、関数を呼び出すコードは、その呼び出しから単一の戻り値を「引き出す」ことによってそれを消費しています。

> [!NOTE]
> ES2015は、別のタイプのプルシステムであるジェネレータ関数とイテレータ（function*）を導入しました。Iterator.next() を呼び出すコードはコンシューマーであり、イテレータ (プロデューサー) から複数の値を「引き出す」。


### Push
プッシュシステムでは、プロデューサーはいつコンシューマーにデータを送信するかを決定します。コンシューマーは、そのデータをいつ受け取るかを知りません。

### データのプロデューサーとコンシューマーの役割の違い
||プロデューサー|コンシューマ|
|---|---|---|
|Pull|Passive: 要求されたときにデータを生成します。|Active: データが要求される時期を決定します。|
|Push|Active: 独自のペースでデータを生成します。|Passive: 受信したデータに反応します。|

この表は「データの流れ方向によるプロデューサーとコンシューマーの役割の違い」を示しています。データの生成と消費の過程において、「引く（Pull）」と「押す（Push）」の二つのアプローチがあります。

- Pull:  
プロデューサーは受動的で、コンシューマーがデータを要求するまで待機します。コンシューマーは能動的にデータを要求します。
- Push:  
プロデューサーは能動的にデータを生成し、コンシューマーは受動的にデータを受け取ります。

## 基本的な特性

- Lazy:  
Observableは、Observerによって購読されるまで値の生成や送出を開始しません。これは、必要になるまで計算リソースを消費しないということを意味します。
- Unicast:  
標準的なObservableはユニキャストです。つまり、Observableは購読ごとに個別の実行を行い、各購読者に対して個別のデータストリームを提供します。

## Observableの作成

ObservableはrxjsモジュールのObservableクラスを用いて作成します。Observableを作成する際には、new Observable()の引数にsubscriber functionを提供します。この関数は、Observableが購読されたときに実行され、データの生成と送出、エラーの通知、完了の通知を行います。

### 実装例１

以下は、単純なObservableの作成と購読の例です：

```js
import { Observable } from 'rxjs';

const observable = new Observable(subscriber => {
  subscriber.next('Hello');
  subscriber.next('World');
  setTimeout(() => {
    subscriber.next('From RxJS');
    subscriber.complete();  // ストリームの完了を通知
  }, 1000);
});

observable.subscribe({
  next(x) { console.log(x); },
  error(err) { console.error('エラー: ' + err); },
  complete() { console.log('完了'); }
});
```
```
Hello
World
From RxJS
完了
```

この例では、Observableが三つの値を非同期に送出し、完了します。subscribeメソッドを使ってObservableを購読し、Observerオブジェクトを提供することで、データの到着、エラー、完了をハンドリングします。

### オペレーター

RxJSの強力な特徴の一つに、データストリームを操作するための多数のオペレーターがあります。これらはpipe()メソッドを通じて使用され、データを変換したり、フィルタリングしたり、他のストリームと組み合わせたりできます。例えば、map(), filter(), merge(), concat()などがあります。

### エラーハンドリング

Observable内でエラーが発生した場合、errorメソッドを通じてエラーがObserverに通知されます。一度エラーが通知されると、Observableはそれ以上の値を送出しません。

Observableは、非同期処理をエレガントに扱うための強力なツールであり、RxJSを用いたリアクティブプログラミングの基盤を成します。

### 実装例２

以下は、サブスクライブ時に値1、3をすぐに（同期的に）プッシュし、サブスクライブ呼び出しから1秒が経過した後、値4が完了するオブザーバブルです。

```js
import { Observable } from 'rxjs';

const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  setTimeout(() => {
    subscriber.next(4);
    subscriber.complete();
  }, 1000);
});

// Observable を呼び出してこれらの値を確認するには、購読する必要があります。
console.log('just before subscribe');
observable.subscribe({
  next(x) {
    console.log('got value ' + x);
  },
  error(err) {
    console.error('something wrong occurred: ' + err);
  },
  complete() {
    console.log('done');
  },
});
console.log('just after subscribe');
```

```
just before subscribe
got value 1
got value 2
got value 3
just after subscribe
got value 4
done
```


## 複数の値を非同期に送出する Observable

オブザーバブルは、同期または非同期で値を提供できます。

### 関数は1つの値しか返せない。
```js
function foo() {
  console.log('Hello');
  return 42;
  return 100; //デッドコード。決して実行されない。
}

console.log(foo());
```
#### 実行結果
```
Hello
42
```

### Observablesは複数の値を返す

```js
import { Observable } from 'rxjs';

const foo = new Observable((subscriber) => {
  console.log('Hello');
  subscriber.next(42);
  subscriber.next(100); // "return" another value
  subscriber.next(200); // "return" yet another
});

console.log('before');
foo.subscribe((x) => {
  console.log(x);
});
console.log('after');
```
#### 実行結果
```
before
Hello
42
100
200
after
```

### Observablesは複数の非同期に値を返す

非同期に値を「返す」こともできます。

```js
import { Observable } from 'rxjs';

const foo = new Observable((subscriber) => {
  console.log('Hello');
  subscriber.next(42);
  subscriber.next(100);
  subscriber.next(200);
  setTimeout(() => {
    subscriber.next(300); // happens asynchronously
  }, 1000);
});

console.log('before');
foo.subscribe((x) => {
  console.log(x);
});
console.log('after');
```
#### 実行結果
```
before
Hello
42
100
200
after
300
```

## Observable を作成する

```js
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  const id = setInterval(() => {
    subscriber.next('hi');
  }, 1000);
});

```
## Observablesを購読する
observable、次のように購読できます。

```js
observable.subscribe((x) => console.log(x));
```
Observable にサブスクライブすることは、関数を呼び出すようなもので、データが配信される場所にコールバックを提供します。

#### 実行結果
```
hi
hi
hi
hi
.
.
```

## Observer の実行

RxJSのObserverは、Observableが送出するデータを消費するためのインターフェースです。Observerは、Observableからデータや通知を受け取る際に使用される一連のコールバック関数を提供します。主に以下の3つのメソッドから構成されています

1. next:  
Observableが新しいデータを送出するたびに呼ばれる関数です。このメソッドは、Observableが生成するデータを受け取り、そのデータに基づいて何らかのアクションを実行します。
2. error:  
Observable内でエラーが発生した場合に呼ばれる関数です。このメソッドはエラーオブジェクトを引数として受け取り、エラーのハンドリングを行います。errorが呼ばれた時点でObservableは完了し、それ以上のデータ送出は行われません。
3. complete:  
Observableが完了した（これ以上送出するデータがない）ことを示すために呼ばれる関数です。このメソッドはパラメータを取らず、購読のクリーンアップなどの終了処理を行うために使用されます。

### Observerの使用例

以下は、RxJSのObserverを使用した簡単な例です：

```js
import { Observable } from 'rxjs';

// Observableを作成
const observable = new Observable(subscriber => {
  try {
    subscriber.next(1);
    subscriber.next(2);
    subscriber.next(3);
    setTimeout(() => {
      subscriber.next(4);
      subscriber.complete();
    }, 1000);
    subscriber.next(5);
  } catch (err) {
    subscriber.error(err);
  }
});

// Observerを定義
const observer = {
  next: x => console.log('次の値: ' + x),
  error: err => console.error('エラー: ' + err),
  complete: () => console.log('完了'),
};

// Observableを購読
observable.subscribe(observer);


```

#### 実行結果
```
次の値: 1
次の値: 2
次の値: 3
次の値: 5
次の値: 4
完了
```


この例では、Observableが複数の値を送出し、最後に完了します。それぞれの値はObserverのnextメソッドによって受け取られ、完了時にはcompleteメソッドが呼ばれます。もしObservable内でエラーが発生した場合には、errorメソッドが呼ばれます。

Observerは、データストリームの消費の柔軟性と明確なエラーハンドリング、完了処理の方法を提供し、RxJSの非同期処理の強力なサポートの核となる部分です。

## Observable　の購読破棄

Observable.subscribeは、ObservableにObserverを接続し、データストリームを購読するメソッドです。購読することでデータの送出を開始し、Observerのnext、error、completeメソッドが呼び出される条件を満たします。

subscribeメソッドが呼び出されると、Observerは新しく作成されたObservable実行にアタッチされます。またこの呼び出しによって、Subscriptionオブジェクトを返します。
```js
const subscription = observable.subscribe((x) => console.log(x));
```

購読を停止するには、subscribeメソッドが返すSubscriptionオブジェクトのunsubscribeメソッドを呼び出します。これにより、リソースの解放や、不要なデータの受信を停止することができます。Subscriptionは購読の管理を容易にするためのもので、複数の購読をグループ化して一括で解除することも可能です。

Subscriptionは進行中の実行を表し、その実行をキャンセルできる最小限のAPIを備えています。unsubscribe()を使用すると、進行中の実行をキャンセルできます。

```js
import { from } from 'rxjs';

const observable = from([10, 20, 30]);
const subscription = observable.subscribe((x) => console.log(x));
// Later:
subscription.unsubscribe();
```

## カスタム subscribe
各Observableは、create()を使用してObservableを作成するときに、その実行のリソースを処分する方法を定義する必要があります。関数 subscribe() からカスタム購読解除関数を返すことで、これを行うことができます。

たとえば、setInterval で間隔実行セットをクリアする方法は次のとおりです。

```js
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  const intervalId = setInterval(() => {
    subscriber.next('hi');
  }, 1000);

  return function unsubscribe() {
    clearInterval(intervalId);
  };
});

const subscription = observable.subscribe({ next: x => console.log(x) });
setInterval(() => {
  subscription.unsubscribe();
}, 5000);

```

#### 実行結果
```
hi
hi
hi
hi
hi
```