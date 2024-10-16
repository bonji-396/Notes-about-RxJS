# Observer

Observerは、Observableによって提供される価値のコンシューマー(消費者)です。Observerは、Observableからデータや通知を受け取る際に使用される一連のコールバック関数を提供します。
主に以下の3つのメソッドから構成されています

- next:  
Observableが新しいデータを送出するたびに呼ばれる関数です。このメソッドは、Observableが生成するデータを受け取り、そのデータに基づいて何らかのアクションを実行します。
- error:  
Observable内でエラーが発生した場合に呼ばれる関数です。このメソッドはエラーオブジェクトを引数として受け取り、エラーのハンドリングを行います。errorが呼ばれた時点でObservableは完了し、それ以上のデータ送出は行われません。
- complete:  
Observableが完了した（これ以上送出するデータがない）ことを示すために呼ばれる関数です。このメソッドはパラメータを取らず、購読のクリーンアップなどの終了処理を行うために使用されます。


## Observerオブジェクト例

Observerは、Observableが配信する可能性のある通知の種類ごとに1つずつ、3つのコールバックを持つオブジェクトにすぎません。
```js
const observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};
```

以下のように、completeコールバックのないObserverなど、コールバックを提供しないObserverのケースもあります。今場合、コールバックがないため、無視されます。
```js
const observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
};
```

オブザーバーを使用するには、オブザーバーのsubscribe者に提供してください。
```js
observable.subscribe(observer);
```

## Observerをアタッチしない方法

オブザーバーブルを購読する場合、オブザーバーオブジェクトにアタッチせずに、次のコールバックを引数として提供することもできます。

```js
observable.subscribe(x => console.log('Observer got a next value: ' + x));
```