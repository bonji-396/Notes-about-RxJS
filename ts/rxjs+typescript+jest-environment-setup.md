# RxJS + TypeScript + Jest の 環境構築


### 1. プロジェクトの初期化
```sh
% mkdir rxjs-jest-ts
% cd rxjs-jest-ts
% npm init -y
```

### 2. TypeScript / Jest / RxJS 関連のパッケージをインストール
```sh
npm install --save rxjs
npm install --save-dev typescript ts-jest @types/jest jest
```

### 3. TypeScript 設定ファイルの作成
```sh
npx tsc --init
```

#### tsconfig.json
生成された tsconfig.json を以下のように調整します
```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "outDir": "./dist"
  },
  "include": ["src", "tests"]
}

```

### 4. Jest の設定ファイルを作成
```ts
npx ts-jest config:init
```

```js
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/tests'],
};
```

### 5. package.json
以下を追加
```json
  "scripts": {
    "test": "jest"
  },
```

### 6. src, tests ディレクトリの設置
```sh
mkdir src tests
```

### 7. src/sample.ts
```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

export const double = (input: number) => {
  return of(input).pipe(map((x) => x * 2));
};
```

### 8 tests/sample.test.ts
```ts
import { double } from '../src/sample';

test('double should multiply by 2', (done) => {
  double(5).subscribe((value) => {
    expect(value).toBe(10);
    done();
  });
});
```

### 9. テスト実行
```sh
% npm test    

> rxjs-jest-ts@1.0.0 test
> jest

 PASS  tests/sample.test.ts
  ✓ double should multiply by 2 (2 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.849 s, estimated 1 s
Ran all test suites.
```