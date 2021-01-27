## TypeScript | JavaScript 導入（Mac）

### はじめに
人気。

### コンパイラ、トランスパイラ
Node.js のバージョンを要確認。  
JavaScript の性質上、バンドラーを使って外部モジュールを取り込む（というかバンドルする）ことは可能。

### TypeScript の型補完
IntelliSense 的なのはないので型チェックのみ。  
ローカル環境が必須。

### 計算量
計算量を明記したリファレンスページがない。  
Spec に実装が書かれているので、計算量に迷ったら読む（しかないと思う）  
[ECMA-262 - Ecma International(https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)

### インストール
```sh
yarn add -D typescript ts-node @types/node
```

### 標準入力
TypeScript | JavaScript というか Node.js に標準入力がない。  
こういうのを作っておく。

```typescript
const scanf = (): number[] =>
  fs
    .readFileSync('/dev/stdin', 'utf8')
    .trim()
    .split(' ')
    .map((x) => +x)
```

ts-node ＋ Mac 環境で動かすと、上のコードでエラーが起こるため、tsc して JavaScript で読み込むか、Linux コンテナ経由で実行する必要がある（調査中）  
また、EOF しないので、<kbd>control + d</kbd> するまで結果が出ない。

### やってみる
VSCode を起動し、`index.ts` を作って以下をコピペ。  
（入力された数値を出すだけのコード）

```typescript
import * as fs from 'fs'

const scanf = (): number[] =>
  fs
    .readFileSync('/dev/stdin', 'utf8')
    .trim()
    .split(' ')
    .map((x) => +x)

const main = (): void => {
  const N = scanf()
  console.log(`${N.join('')}`)
}

main()
```

`./node_modules/.bin/ts-node ./src/sample.ts` を実行して動けば OK。

### Jest での TDD なやり方
入出力にクセがありすぎるので、Jest などを入れて TDD 的にやった方がいいかも（TBD）