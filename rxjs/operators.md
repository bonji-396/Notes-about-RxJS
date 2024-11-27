# さまさまなOperator

## Creation Operators

|operator|概要説明|
|---|---|
|ajax|URL、ヘッダーなどのリクエストオブジェクトまたはURLの文字列を使用して、Ajaxリクエストのオブザーバーを作成します。|
|bindCallback|コールバック API を Observable を返す関数に変換します。|
|bindNodeCallback|Node.jsスタイルのコールバックAPIをObservableを返す関数に変換します。|
|defer|オブザーブルを作成し、サブスクライブ時にオブザーブルファクトリを呼び出して、新しいオブザーバーごとにオブザーブルを作成します。|
|~~empty~~|EMPTY定数またはscheduled（例：scheduled([], scheduler)）に置き換えます。v8で削除されます。|
|from|配列、配列のようなオブジェクト、Promise、反復可能なオブジェクト、またはObservableのようなオブジェクトからObservableを作成します。|
|fromEvent||
|fromEventPattern||
|generate||
|interval||
|of||
|range||
|throwError||
|timer||
|iif||


## Join Creation Operators
multiple source Observables値を放出する、結合機能も備えたObservables作成演算子です

|operator|概要説明|
|---|---|
|combineLatest||
|concat||
|forkJoin||
|merge||
|partition||
|race||
|zip||


## Transformation Operators

|operator|概要説明|
|---|---|
|buffer||
|bufferCount||
|bufferTime||
|bufferToggle||
|bufferWhen||
|concatMap||
|concatMapTo||
|exhaust||
|exhaustMap||
|expand||
|groupBy||
|map||
|mapTo||
|mergeMap||
|mergeMapTo||
|mergeScan||
|pairwise||
|partition||
|pluck||
|scan||
|switchScan||
|switchMap||
|switchMapTo||
|window||
|windowCount||
|windowTime||
|windowToggle||
|windowWhen||


## Filtering Operators

|operator|概要説明|
|---|---|
|audit||
|auditTime||
|debounce||
|debounceTime||
|distinct||
|distinctUntilChanged||
|distinctUntilKeyChanged||
|elementAt||
|filter||
|first||
|ignoreElements||
|last||
|sample||
|sampleTime||
|single||
|skip||
|skipLast||
|skipUntil||
|skipWhile||
|take||
|takeLast||
|takeUntil||
|takeWhile||
|throttle||
|throttleTime||

## Join Operators

|operator|概要説明|
|---|---|
|combineLatestAll||
|concatAll||
|exhaustAll||
|mergeAll||
|switchAll||
|startWith||
|withLatestFrom||


## Multicasting Operators

|operator|概要説明|
|---|---|
|multicast||
|publish||
|publishBehavior||
|publishLast||
|publishReplay||
|share||

## Error Handling Operators

|operator|概要説明|
|---|---|
|catchError||
|retry||
|retryWhen||

## Utility Operators

|operator|概要説明|
|---|---|
|tap||
|delay||
|delayWhen||
|dematerialize||
|materialize||
|observeOn||
|subscribeOn||
|timeInterval||
|timestamp||
|timeout||
|timeoutWith||
|toArray||

## Conditional and Boolean Operators

|operator|概要説明|
|---|---|
|defaultIfEmpty||
|every||
|find||
|findIndex||
|isEmpty||

## Mathematical and Aggregate Operators

|operator|概要説明|
|---|---|
|count||
|max||
|min||
|reduce||
