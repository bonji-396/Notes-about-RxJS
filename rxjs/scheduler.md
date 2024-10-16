# Schedulers
Scheduler は、Observable が値を発行するタイミングや方法を制御するためのコンポーネントです。Scheduler を使用することで、Observable の操作や発行される値の配信を特定の実行コンテキスト（例：即時実行、非同期実行、アニメーションフレーム等）にスケジュールすることができます。これは、RxJS において非同期処理や時間に依存した処理を柔軟に扱うために非常に重要な機能です。


## Scheduler の種類

### null (デフォルト)
特定の Scheduler を指定しない場合、Observable は同期的に動作します。これは、Scheduler が指定されていない場合のデフォルトの振る舞いです。

デフォルトでは、スケジューラを指定しない場合、Observableは同期的に実行されます。
```js
import { of } from 'rxjs';

console.log('Before subscription');
of(1, 2, 3).subscribe(value => console.log(value));
console.log('After subscription');
// 出力:
// Before subscription
// 1
// 2
// 3
// After subscription
```

### queueScheduler
タスクをキューに追加し、現在のタスクやイベントが完了した後に実行されるようスケジュールします。これは、再帰的なアクションがスタックオーバーフローを引き起こすことなくスケジュールされるようにするために使用されます。

キュースケジューラは、現在のイベントループ/タスクが完了した直後に実行をスケジュールします。
```js
import { scheduled, queueScheduler, of } from 'rxjs';

console.log('Before subscription');
scheduled(of(1, 2, 3), queueScheduler).subscribe(value => console.log(value));
console.log('After subscription');
// 出力:
// Before subscription
// 1
// 2
// 3
// After subscription
```

### asapScheduler
マクロタスク（例えば setTimeout や setInterval）よりも早く、しかし現在実行中のタスクが完了した直後に実行されるマイクロタスクキューにタスクをスケジュールします。非常に小さな遅延後に操作を行いたい場合に適しています。

マイクロタスクキューを利用して、できるだけ早く実行しますが、現在の同期的な実行が終了した後に行われます。

```js
import { scheduled, asapScheduler, of } from 'rxjs';

console.log('Before subscription');
scheduled(of(1, 2, 3), asapScheduler).subscribe(value => console.log(value));
console.log('After subscription');
// 出力:
// Before subscription
// After subscription
// 1
// 2
// 3
```

### asyncScheduler
非同期のイベント（例えば setTimeout）を使用して、タスクをスケジュールします。指定された時間遅延での実行に使用されることが多いです。

タスクを非同期的にスケジュールし、デフォルトでは setTimeout を使用して実行を遅延します。
```js
import { scheduled, asyncScheduler, of } from 'rxjs';

console.log('Before subscription');
scheduled(of(1, 2, 3), asyncScheduler).subscribe(value => console.log(value));
console.log('After subscription');
// 出力:
// Before subscription
// After subscription
// 1
// 2
// 3
```

### animationFrameScheduler
ブラウザのアニメーションフレームにタスクをスケジュールします。画面の描画と同期をとるため、アニメーションや UI の更新に最適です。

ブラウザのアニメーションフレームに合わせてタスクをスケジュールします。主にビジュアルアップデートに最適です。

```js
import { scheduled, animationFrameScheduler, of } from 'rxjs';

console.log('Before subscription');
scheduled(of(1, 2, 3), animationFrameScheduler).subscribe(value => console.log(value));
console.log('After subscription');
// 出力:
// Before subscription
// After subscription
// (アニメーションフレームに合わせた実行タイミングで 1, 2, 3 が出力される)
```