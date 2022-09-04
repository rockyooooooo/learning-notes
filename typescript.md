# TypeScript

## Install

```zsh
# globally
npm install -g typescript

# in project
npm install typescript --save-dev
```

接下來就可以用以下的指令來執行 typescript：

```zsh
# installed globally
tsc <source> <destination>

# installed in project
npx tsc <source> <destination>
```

不輸入 destination 的話，會直接產生一支跟 source 同名的 `.js` 在相同目錄底下。

如果不想要每次修改完 `.ts` 檔就要執行一次 `tsc` 來編譯成 `.js` 的話，可以加上 `-w` 這個 option 來監聽。

## tsconfig.json

執行 `tsc --init` 來產生 `tsconfig.json`。

編輯 `tsconfig.json`

```json
{
  "compilerOptions": {
    ...
      "rootDir": "./src",
      "outDir": "./public",
    ...
  }
}
```

如此一來直接執行 `tsc -w` 就可以監聽整個 `src/`，並把 compile 完的結果輸出到 `public/`。

不過這時候如果在 `src/` 之外新增一個 `.ts` 檔案，會發現他一樣會 compile 他，且在 terminal 噴以下 error：

```zsh
error TS6059: File '<dir_path>/app.ts' is not under 'rootDir' '<dir_path>/src'. 'rootDir' is expected to contain all source files.
  The file is in the program because:
    Matched by default include pattern '**/*'
```

要再加上：

```json
{
  ...
  "include": [
    "src"
  ]
  ...
}
```

如此一來，就只會監聽 `src/` 這個資料夾了。

## Declaring Variables

### Implicit

```js
let username = 'allen'
username = 28 // Type 'number' is not assignable to type 'string'.

/* 甚至一開始設成 null 或 undefined 之後，就再也不能設成其他 value 了 */
let temp = null
temp = 'yo' // Type '"yo"' is not assignable to type 'null'.

/* 如果沒有設 default value 的話，就會是 any type */
let test // Variable 'test' implicitly has an 'any' type, but a better type may be inferred from usage.
test = 'sting' // ok
test = 99 // ok

/* Array 一開始裡面有哪些 type，之後不能有別的 type 出現 */
const numbers = [1, 2, 3, 4] // numbers: number[]
numbers.push(5)
numbers.push('allen') // Argument of type 'string' is not assignable to parameter of type 'number'.ts
numbers[0] = 'allen' // Type 'string' is not assignable to type 'number'.ts

const mixed = [1, 2, 'allen'] // mixed: (string | number)[]
mixed.push(3)
mixed[2] = 'yo'
mixed.push(true) // Argument of type 'boolean' is not assignable to parameter of type 'string | number'.

/* Object 的 property 一開始的 value 是什麼 type 就只能是那個 type，有哪些 properties 就一個也不能少，多一個也不行 */
let person = {
  name: 'allen',
  age: 28
}

person.name = 10 // Type 'number' is not assignable to type 'string'.
person = {
  name: 'liao'
} // Property 'age' is missing in type '{ name: string; }' but required in type '{ name: string; age: number; }'.
person = {
  name: 'yo',
  age: 30,
  skills: ['coding', 'liān-siáu-uē']
} // Type '{ name: string; age: number; skills: string[]; }' is not assignable to type '{ name: string; age: number; }'. Object literal may only specify known properties, and 'skills' does not exist in type '{ name: string; age: number; }'.
```

### Explicit

```js
let username: string
let age: number
let inLoggedIn: boolean
let uuid: string|number // union types
let people: string[] // array of string
let mixed: (string|number)[] = [] // can be union types and set to empty array by default (Don't forget the parentheses)
let person: {
  name: string,
  age: number,
  isLoggedIn: boolean
} = {}

/* 把 typescript 打回 javascript */
let age: amy
let mixed: any[]
```

## Function

一樣宣告成了 function 就不能再設成別的 type。

```js
/* implicit */
let greet = () => {
  console.log('hello')
}

/* explicit */
let greet: Function // with capital `F`

greet = 'hello' // Type 'string' is not assignable to type '() => void'.
```

宣告 function parameters 的 type：

```js
const circ = (diameter: number) => diameter * Math.PI
circ('hello') // Argument of type 'string' is not assignable to parameter of type 'number'.

const add = (a: number, b: number, c?: string | number) => a + b // 加上 ? 表示 optional parameter
add(1, 2) // ok

/* function return 的值 assign 到變數，變數的 type 也會被限制 */
const minus = (a: number, b: number, c?: string | number) => a - b
let result = minus(2, 1)
result = 'yo' // Type 'string' is not assignable to type 'number'.

/* 因為已經宣告 bar 是字串，所以在 function 裡面判斷 bar === 1 時，typescript 會檢查到這個 if 永遠是 false */
const foo = (bar: string) => {
  if (bar === 1) return true // This condition will always return 'false' since the types 'string' and 'number' have no overlap.ts
  return false
}

```

## Type Alias

可以把 type 存成 alias 來重複使用：

```js
type stringOrNum = string | number
type objWithName = { name: string, uuid: stringOrNum }
const greet = (user: objWithName) => {
  console.log(`${user.name} says hello`)
}
```

## Function signatures

```js
let greet: (a: string, b: string) => void // a 跟 b 可以改成任何你喜歡的字
greet = (name, greeting) => { // 這邊的 name 跟 greeting 不用叫做 a 跟 b
  console.log(`${name} says ${greeting}`)
}
```
