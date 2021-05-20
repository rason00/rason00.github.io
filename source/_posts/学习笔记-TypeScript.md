---
title: 学习笔记-TypeScript
date: 2021-5-20 21:58:43
tags: TypeScript
categories: 学习笔记
---

# TypeScript-基础类型

入门级了解`TypeScript`基础类型，根据网上查询的资料进行学习及记录以下学习笔记。

<!-- more -->

## 一、基础类型

### 1. 布尔（boolean）

```ts
/* 布尔（boolean） */
let isDone: boolean = true;
```

### 2. 数字（number）

```ts
/* 数字（number） */
let listIndex: number = 10;	// 支持十进制、十六进制、二进制和八进制字面量
```

### 3. 字符串（string）

```ts
/* 字符串（string） */
let name: string = 'Rason';	// 支持 ‘`’ 反引号加变量，例：`hello ${ name }`
```

### 4. 数组（元素类型[] || Array<元素类型>

```ts
/* 数组（元素类型[] || Array<元素类型>） */
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3]
```

### 5. 元组（[元素类型, 元素类型]）

```ts
/* 元组（[元素类型, 元素类型]） */
let x: [number, string] = [1, 'test'];
```

### 6. 枚举（enum）

```ts
/* 枚举（enum） */
enum Color {red, green, blue};	// 可手动赋值编号。例：enum Color {red = 2, green};
let c: Color = Color.green;		// 可由编号得它的名字，例：let cName: string = Color[2];
```

### 7. 任意值（Any）

```ts
/* 不确定类型（Any） */
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

### 8. 空值（Void）

当一个函数没有返回值时，你通常会见到其返回值类型是 void

```ts
/* 无类型（Void） */
function testFunction(): void {
    console.log('This is a test function');
}

// 声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null
let unusable: void = undefined;
```

### 9. Null 和 Undefined

```ts
/* Null 和 Undefined */
let u: undefined = undefined;
let n: null = null;
```

### 10. Never

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。通常表现为抛出异常或无法执行到终止点（例如无线循环）

```ts
/* Never */
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无限循环这种无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```

### 11. Object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

```ts
/* object */
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 12. 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

```ts
// 类型断言有两种形式。 其一是“尖括号”语法：
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// 另一个为 as 语法：
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

> 两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好。
>
> 然而，当你在`TypeScript`里使用`JSX`时，只有 `as`语法断言是被允许的！！！

### 13. 联合类型

联合类型（`Union Types`）表示取值可以为多种类型中的一种。

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

### 14. 类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（`Type Inference`）的规则推断出一个类型。

```ts
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
// 报错：error TS2322: Type 'number' is not assignable to type 'string'.

// 它等价于：
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

```

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**：

```ts
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 参考

- [Basic Types](http://www.typescriptlang.org/docs/handbook/basic-types.html)（[基础类型（中文版）](https://www.tslang.cn/docs/handbook/basic-types.html)）
- [Union Types](<http://www.typescriptlang.org/docs/handbook/advanced-types.html>)（[联合类型（中文版）](<https://www.tslang.cn/docs/handbook/advanced-types.html>)）
- [Type Inference](http://www.typescriptlang.org/docs/handbook/type-inference.html)（[类型推论（中文版）](https://www.tslang.cn/docs/handbook/type-inference.html)）
