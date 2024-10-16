# マーブルダイアグラムでコードをテストする

RxJS コードのテストにおいて、マーブルダイアグラムは非常に役立つツールです。これは、時間の経過とともに Observable がどのように値を発行するかを視覚的に表現したものです。マーブルダイアグラムを利用することで、複雑な非同期処理や時間に依存するデータストリームを直感的かつ正確にテストすることができます。

マーブルダイアグラムの基本

マーブルダイアグラムでは、各マーブル（円やその他の記号）が Observable からの値の発行を表し、時間は左から右へと進行します。また、特定の記号はエラー発生や Observable の完了を示すために使われます。

テストにおけるマーブルダイアグラムの利用

RxJS のテストでは、TestScheduler クラスを使用して仮想時間を制御し、マーブルダイアグラムを用いて Observable の挙動を模倣（モック）します。これにより、非同期処理を同期的にテストでき、Observable の期待される出力を時間の経過と共に確認できます。

基本的なマーブルダイアグラムの例

以下は、RxJS のテストで TestScheduler を使って単純なマーブルダイアグラムを用いたテストの一例です。
```js
import { TestScheduler } from 'rxjs/testing';
import { map } from 'rxjs/operators';

describe('RxJS Testing', () => {
  let testScheduler;

  beforeEach(() => {
    // TestScheduler のインスタンスを作成し、テストに利用する。
    testScheduler = new TestScheduler((actual, expected) => {
      expect(actual).toEqual(expected);
    });
  });

  it('should map values', () => {
    testScheduler.run(helpers => {
      const { cold, expectObservable } = helpers;
      const inputMarbles = '-a-b-c|';
      const expectedMarbles = '-x-y-z|';

      const inputValues = { a: 1, b: 2, c: 3 };
      const expectedValues = { x: 2, y: 4, z: 6 };

      const source = cold(inputMarbles, inputValues).pipe(
        map(x => x * 2)
      );

      expectObservable(source).toBe(expectedMarbles, expectedValues);
    });
  });
})
```

この例では、inputMarbles で定義された時間ごとに 1, 2, 3 の値を発行し、それを map 演算子でそれぞれ 2, 4, 6 に変換し、期待される出力 expectedMarbles に一致するかをテストしています。

利点

	1.	明確な視覚表現: マーブルダイアグラムを使うことで、Observable の挙動が一目でわかります。
	2.	正確なタイミングのテスト: 仮想時間を操作することで、非常に精密なタイミングでのテストが可能になります。
	3.	エラーと完了のハンドリング: エラーや完了もダイアグラム上で表現でき、それらの挙動を簡単にテストできます。

マーブルダイアグラムは、RxJS を使ったアプリケーションの開発において、特に複雑な非同期処理やエラーハンドリングをテストする際に非常に有効なツールです。