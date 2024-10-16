# Operator

Operatorは、Observableが生成するデータストリームを操作する関数です。  
これらはデータの変換、フィルタリング、結合などを行うために用いられます。  

## Operatorの種類
オペレーターには2種類あります

- Pipeable Operator:  
pipe()関数と共に使用され、Observableを入力として受け取り、
別のObservableを出力として生成するOperators
- Creation Operators:  
Observableが生成するOperators

## Pipeable Operator
Pipeable operatorsは、データストリームを操作するためにpipe()関数と共に使用されるRxJSの関数です。
`observableInstance.pipe(operator)`または、より一般的には`observableInstance.pipe(operatorFactory())`の構文を使用してObservablesにパイプできる種類です。

### pipe演算子
Pipeable演算子は本質的に、1つのObservableを入力として受け取り、別のObservableを出力として生成する純粋な関数です。以前のObservableは変更されません。出力Observableを購読すると、入力Observableも購読されます。


### map
入力値に関数を適用し、その結果を新しい値として出力します。  
例: `source.pipe(map(x => x * 2))` は、ソースの各値を2倍にします。

```js
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);
const doubled = source.pipe(map(value => value * 2));
doubled.subscribe(console.log);  // 出力: 2, 4, 6, 8, 10
```

### filter
条件に基づいて値をフィルタリングします。  
例: `source.pipe(filter(x => x > 3))` は、3より大きい値だけを通過させます。
```js
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);
const filtered = source.pipe(filter(value => value > 3));
filtered.subscribe(console.log);  // 出力: 4, 5
```

### tap
副作用（デバッグログ、値の確認など）のために値を観察しますが、ストリーム自体には影響を与えません。  
例: `source.pipe(tap(x => console.log(x)))` は、各値をコンソールにログします。

```js
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

const source = of('a', 'b', 'c');
source
  .pipe(
    tap(value => console.log(`Before: ${value}`)),
    map(value => value.toUpperCase()),
    tap(value => console.log(`After: ${value}`))
  )
  .subscribe();
// 出力:
// Before: a
// After: A
// Before: b
// After: B
// Before: c
// After: C
```

### take
指定した数の値を取得した後、Observableを完了させます。  
例: `source.pipe(take(3))` は、最初の3つの値の後で完了します。
```js
import { of } from 'rxjs';
import { take } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);
const taken = source.pipe(take(3));
taken.subscribe(console.log);  // 出力: 1, 2, 3
```
### switchMap
入力値に基づいて新しいObservableを生成し、以前のObservableを自動的に解約します。  
例: ユーザーの入力ごとに新しいHTTPリクエストを発行し、前のリクエストをキャンセルする。
```js
import { fromEvent } from 'rxjs';
import { switchMap } from 'rxjs/operators';

const button = document.createElement('button');
button.textContent = 'Click me';
document.body.appendChild(button);

fromEvent(button, 'click').pipe(
  switchMap(() => fetch('https://jsonplaceholder.typicode.com/todos/1').then(response => response.json()))
).subscribe(console.log);
// 'Click me'ボタン押下時の出力:
// {userId: 1, id: 1, title: "delectus ...}
```
### catchError
エラーを捕捉して、それに対処するためのObservableを提供します。  
例: エラーが発生した場合にデフォルト値を返す。
```js
import { of, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

const source = throwError(new Error('Something went wrong!'));
const handled = source.pipe(
  catchError(error => of(`Caught: ${error.message}`))
);
handled.subscribe(console.log);  // 出力: Caught: Something went wrong!
```
### debounceTime
指定された時間内に連続した値の最後のものだけを出力します。  
例: ユーザーの入力を待ってからHTTPリクエストを発行する。
```js
import { fromEvent } from 'rxjs';
import { debounceTime, map } from 'rxjs/operators';

const input = document.createElement('input');
document.body.appendChild(input);

fromEvent(input, 'input').pipe(
  map(event => event.target.value),
  debounceTime(300)
).subscribe(console.log);
```


## Piping
Pipeable Operatorは関数であるため、通常の関数のように使用できます。

`op()(obs)` - しかし、実際には、それらの多くが一緒に巻き合わされ、すぐに読めなくなる傾向があります: `op4()(op3()(op2()(op1()(obs)))))`。そのため、Observables には `.pipe()` というメソッドがあり、読みやすくなりながら同じことを達成します。

```js
obs.pipe(op1(), op2(), op3(), op4());
```
演算子が1つしかない場合でも、op()(obs)は決して使用されません。obs.pipe(op())は普遍的に好まれます。

## Pipeable Operator Factory
Pipeable Operator Factoryは、パラメータを取り、コンテキストを設定し、Pipeable Operatorを返すことができる関数です。ファクトリの引数は、Operatorの語彙範囲に属します。

RxJS の Pipeable Operator Factory は、パラメータに基づいて動的にオペレーターを生成する関数です。これにより、カスタマイズ可能で再利用可能なオペレーターを作成することができ、特定の条件に基づいてデータを変換またはフィルタリングする処理を柔軟に実装できます。

### 基本的な概念

Operator Factory は関数を返す関数です。返された関数は、Observable を引数に取り、新しい Observable を返します。これにより、特定の論理や条件に基づいて、カスタマイズされた操作をデータストリームに適用できます。

例: カスタム Pipeable Operator Factory

以下は、RxJS の Pipeable Operator Factory の一例で、filter オペレーターの動作をカスタマイズするために使われます。

```js
import { pipe } from 'rxjs';
import { filter } from 'rxjs/operators';

// Operator Factory: 指定された条件関数に基づいてフィルタリングするオペレーターを生成
function filterWithCondition(condition) {
  return function(source) {
    return source.pipe(filter(condition));
  };
}

// 使用例
import { of } from 'rxjs';

const source = of(1, 2, 3, 4, 5);
const filtered = source.pipe(filterWithCondition(x => x > 2));
filtered.subscribe(console.log); // 出力: 3, 4, 5

```
<!-- ```ts
import { pipe, Observable } from 'rxjs';
import { filter } from 'rxjs/operators';

// Operator Factory: 指定された条件関数に基づいてフィルタリングするオペレーターを生成
function filterWithCondition<T>(condition: (value: T) => boolean) {
  return function (source: Observable<T>): Observable<T> {
    return source.pipe(filter(condition));
  };
}

// 使用例
import { of } from 'rxjs';

const source = of(1, 2, 3, 4, 5);
const filtered = source.pipe(filterWithCondition((x) => x > 2));
filtered.subscribe(console.log); // 出力: 3, 4, 5

``` -->


この例では、filterWithCondition は、条件関数をパラメータとして受け取り、その条件に一致する値のみを通過させる新しいオペレーターを生成します。

### 利点
- 再利用性:  
一度定義すれば、様々な条件やパラメータで繰り返し使用できます。
- カスタマイズ性:  
アプリケーションの特定のニーズに合わせて、データストリームの操作を細かく調整できます。
- 保守性:  
コードの変更が必要な場合、Factory だけを更新すれば良いため、影響範囲を限定しやすいです。

このように、Pipeable Operator Factory を使うことで、RxJS を使ったアプリケーションの柔軟性と拡張性を大きく向上させることができます。

## Creation Operators
Creation Operatorsは、様々なソース（特定の値、イベント、プロミスなど）からObservableを生成するための関数です。of、from、intervalなどが含まれます。

```js
import { interval } from 'rxjs';

const observable = interval(1000).subscribe(x => console.log(x));
// 出力:
// 1
// 2
// 3
```


以下に代表的なCreation Operatorsをいくつか紹介します。


### of(...values)
引数として与えられた値からObservableを生成します。値は順番に発行され、完了します。  
例: `of(1, 2, 3)` は 1, 2, 3 という値を順番に発行し、その後完了します。

```js
import { of } from 'rxjs';

const numbers$ = of(1, 2, 3);
numbers$.subscribe(console.log);  // 出力: 1, 2, 3
```

### from(iterable|promise|observable):
配列やIterable、Promise、他のObservableなどからObservableを生成します。
例: `from([1, 2, 3]) `は配列の各要素を順番に発行します。

```js
import { from } from 'rxjs';

const array$ = from([10, 20, 30]);
array$.subscribe(console.log);  // 出力: 10, 20, 30
```

### fromEvent(target, eventName):
DOM要素やNode.jsのイベントエミッタから特定のイベントに基づくObservableを生成します。  
例: `fromEvent(document, 'click') `はドキュメント上でのクリックイベントをリッスンします。

```js
import { fromEvent } from 'rxjs';

const clicks$ = fromEvent(document, 'click');
clicks$.subscribe(() => console.log('Document was clicked!'));
// クリック時の出力:
// Document was clicked!
// Document was clicked!
```

### interval(period):
指定された時間間隔ごとに連続する整数を発行するObservableを生成します。  
例: `interval(1000)` は1秒ごとに連続する整数（0から始まる）を発行します。

```js
import { interval } from 'rxjs';

const interval$ = interval(1000);  // 1000ms, または1秒ごと
interval$.subscribe(console.log);  // 0から始まり、1秒ごとにインクリメントされる値を出力
```

### timer(delay, [period]):
指定された遅延後に値を発行し、オプションで定期的に値を発行するObservableを生成します。  
例: `timer(1000) `は1秒後に0を発行し、その後完了します。timer(2000, 500) は2秒後に0を発行し、その後500ミリ秒ごとに値をインクリメントしながら発行します。

```js
import { timer } from 'rxjs';

const timer$ = timer(1000, 500);  // 最初の発行は1秒後、その後500msごと
timer$.subscribe(console.log);
// 出力:
// 1
// 2
// 3
// .
// .
```

### range(start, count):
指定された範囲の連続する整数を発行するObservableを生成します。  
例: `range(1, 3)` は1から始まり3つの値（1, 2, 3）を発行します。

```js
import { range } from 'rxjs';

const range$ = range(1, 3);  // 1から始まり、3つの値を発行（1, 2, 3）
range$.subscribe(console.log);  // 出力: 1, 2, 3
```
