# はじめに
RxJSは、観測可能なシーケンスを使用して非同期およびイベントベースのプログラムを構成するためのライブラリです。非同期イベントをコレクションとして処理できるように、1つのコアタイプ、オブザーバブル、サテライトタイプ（オブザーバー、スケジューラ、サブジェクト）とArrayメソッド（map、filter、reduce、everyなど）に触発されたオペレーターを提供します。

## 概念

- Observable:  
データやイベントのストリームを表し、これらが時間を通じてどのように送出されるかを定義します。Observableは、Observerによって購読されるまで何も送出しません。
- Observer:  
Observableから送出されるデータを受け取るためのコールバックのセットです。next, error, completeという3つの主要なメソッドを持っています。
   - `next`: 新しい値を受け取るコールバック関数。
   - `error`: エラーを処理するオプションのコールバック関数。
   - `complete`: ストリームの完了を処理するオプションのコールバック関数。
- Subscription:  
Observableへの購読を表し、購読を開始し、必要に応じて購読を解除するための方法を提供します。
- Operators:  
ストリームのデータを変換、フィルター、合成するための純粋関数です。例えばmap, filter, merge, concatなどがあります。
- Subject:  
ObservableとObserverの両方の役割を果たす特殊なタイプのObservableで、複数のObserverに対して値をマルチキャストできます。
- Scheduler:  
並行性を制御するための集中型ディスパッチャであり、setTimeoutやrequestAnimationFrameなど、計算がいつ行われるかを調整することができます。


### 主な利点

1. 非同期処理の簡素化:  
RxJSは、時間や非同期処理を簡単に扱うことができるため、コンプレックスな非同期ロジックをシンプルに記述できます。
2. パワフルなオペレーター:  
さまざまなオペレーターを利用することで、データストリームの操作が直感的に行えます。
3. エラーハンドリング:  
Observableチェーン内でエラーを捕捉し、適切に処理することができます。
4. マルチキャストサポート:  
Subjectを使用することで、同じデータストリームを複数の購読者に送信することが可能です。

## 使用例

### 純度(Purity)
#### 通常のコード
通常、コードの他の部分が状態を混乱させる可能性のある不純な関数を作成します。
```js
let count = 0;
document.addEventListener('click', () => console.log(`Clicked ${++count} times`));
```

#### RxJSのコード
RxJSを使用すると、状態を分離できます。
```js
import { fromEvent, scan } from 'rxjs';

fromEvent(document, 'click')
  .pipe(scan((count) => count + 1, 0))
  .subscribe((count) => console.log(`Clicked ${count} times`));
```
スキャン演算子は、配列の削減と同じように機能します。コールバックに公開される値を取ります。コールバックの返された値は、次にコールバックが実行されたときに公開される次の値になりま


### 流れ(Flow)
RxJSには、イベントがオブザーバブルをどのように通過するかを制御するのに役立つさまざまな演算子があります。

#### 通常のコード
1秒間に最大1回のクリックを許可する方法です。

```js
let count = 0;
let rate = 1000;
let lastClick = Date.now() - rate;
document.addEventListener('click', () => {
  if (Date.now() - lastClick >= rate) {
    console.log(`Clicked ${++count} times`);
    lastClick = Date.now();
  }
});
```

#### RxJSのコード
```js
import { fromEvent, throttleTime, scan } from 'rxjs';

fromEvent(document, 'click')
  .pipe(
    throttleTime(1000),
    scan((count) => count + 1, 0)
  )
  .subscribe((count) => console.log(`Clicked ${count} times`));
```


### 価値(Values)
オブザーバブルを通過する値を変換できます。
クリックするたびに現在のマウス x 位置をプレーン JavaScript で追加する方法は次のとおりです。
#### 通常のコード
```js
let count = 0;
const rate = 1000;
let lastClick = Date.now() - rate;
document.addEventListener('click', (event) => {
  if (Date.now() - lastClick >= rate) {
    count += event.clientX;
    console.log(count);
    lastClick = Date.now();
  }
});
```

#### RxJSのコード

```js
import { fromEvent, throttleTime, map, scan } from 'rxjs';

fromEvent(document, 'click')
  .pipe(
    throttleTime(1000),
    map((event) => event.clientX),
    scan((count, clientX) => count + clientX, 0)
  )
  .subscribe((count) => console.log(count));
```
