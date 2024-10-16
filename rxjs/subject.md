# Subject
Subject は、Observable と Observer の両方の性質を持つ特別な種類の Observable です。Observableは、アタッチされたObserver に対してにしかデータを送信できませんでしたが、Subject を使用することで、複数の Observer に対して値をマルチキャスト（一つのデータソースから複数の購読者にデータを送信）することができます。


## Subject の基本的な特徴

以下に Subject の主要な特徴とその種類を説明します。

- マルチキャスト:  
通常の Observable は、各購読に対して個別の実行を行います（ユニキャスト）が、Subject は購読されるたびに新しい実行を開始するのではなく、同じ実行をすべての購読者に共有します。
- Observer としての機能:  
Subject は .next(value)、.error(error)、.complete() メソッドを通じて、外部からデータを発行することができます。これにより、イベントの発生源として機能することができます。
- 状態の保持:  
Subject は現在の状態を保持せず、データが Subject にプッシュされた時点でのみ、そのデータを購読者に送信します。

## Subject の種類

### Basic Subject
最も基本的な形式の Subject で、新しいデータがプッシュされるたびに現在の購読者にそのデータを送信します。
```js
import { Subject } from 'rxjs';

const subject = new Subject();
subject.subscribe(value => console.log(`Observer 1: ${value}`));
subject.subscribe(value => console.log(`Observer 2: ${value}`));

subject.next(1); // Observer 1: 1, Observer 2: 1
subject.next(2); // Observer 1: 2, Observer 2: 2
// 出力:
// Observer 1: 1
// Observer 2: 1
// Observer 1: 2
// Observer 2: 2

```

### BehaviorSubject
初期値を持ち、新しい購読者には最後に発行された値（または初期値）が自動的に送信されます。

```js
import { BehaviorSubject } from 'rxjs';

const behaviorSubject = new BehaviorSubject(0);  // 初期値 0
behaviorSubject.subscribe(value => console.log(`Observer 1: ${value}`));  // 初期値の 0 を受け取る

behaviorSubject.next(1);
behaviorSubject.next(2);
behaviorSubject.subscribe(value => console.log(`Observer 2: ${value}`));  // 最後に発行された値の 2 を受け取る

behaviorSubject.next(3);
// 出力:
// Observer 1: 0
// Observer 1: 1
// Observer 1: 2
// Observer 2: 2
// Observer 1: 3
// Observer 2: 3
```


### ReplaySubject:
指定された数の値を保持し、新しい購読者に対してその数だけ最新の値を送信します。
```js
import { ReplaySubject } from 'rxjs';

const replaySubject = new ReplaySubject(2); // 最後の 2 つの値を保持
replaySubject.next(1);
replaySubject.next(2);
replaySubject.next(3);
replaySubject.subscribe(value => console.log(`Observer1: ${value}`)); // 2 と 3 を受け取る

replaySubject.next(4);
replaySubject.subscribe(value => console.log(`Observer2: ${value}`)); // 3, 4 を受け取る

replaySubject.next(5);
replaySubject.next(6);
// 出力:
// Observer1: 2
// Observer1: 3
// Observer1: 4
// Observer2: 3
// Observer2: 4
// Observer1: 5
// Observer2: 5
// Observer1: 6
// Observer2: 6
```

### AsyncSubject:
complete() メソッドが呼ばれたときのみ、その時点での最後の値を購読者に送信します。

```js
import { AsyncSubject } from 'rxjs';

const asyncSubject = new AsyncSubject();
asyncSubject.subscribe(value => console.log(`Observer1: ${value}`));
asyncSubject.subscribe(value => console.log(`Observer2: ${value}`));

asyncSubject.next(1);
asyncSubject.next(2);
asyncSubject.subscribe(value => console.log(`Observer3: ${value}`));

asyncSubject.complete(); // 2 を受け取る
asyncSubject.next(3);
// 出力:
// Observer1: 2
// Observer2: 2
// Observer3: 2
```