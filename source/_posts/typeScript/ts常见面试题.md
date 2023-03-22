---
title: ts常见面试题
date: 2020-03-10
categories: typescript
---

## 什么是TypeScript？
ts就是一个js的超集，ts不能直接在浏览器上运行，需要转译成js。他给js的变量提供了类型，让代码在编写的时候就能检测错误。让代码更加的健壮。

## ts相对于js的优势？
js在运行的时候才知道哪里错误，ts在编写的时候就可以检测是否有写错误的代码。
避免写变量名的时候写错。
ts写类型，让代码能更好的看懂，比如函数传入的每个参数都是什么类型。
写ts代码的时候会提示该输入什么参数类型的值。
让代码的质量更好，更加的健壮，避免传入的类型不同而引起的问题。

## ts中const和readonly的区别
const使变量不能修改，readonly使将参数变成只读的，避免修改。

## TypeScript 中 any 类型的作用是什么？
如果你不清楚一个值是什么类型就可以设置成any，any类型可以赋值给任何类型，任何类型也可以赋值给any，any被称为顶级类型。
但是any存在一些问题：
1. 类型污染，any类型的对象，会将参数也全部变成any值
2. 完全失去类型检测功能，当我们使用不存在的方法或属性也不会报错。
可以使用unknown来代替any

## TypeScript 中 any、never、unknown、null & undefined 和 void 有什么区别？
any：ny类型可以赋值给任何类型，任何类型也可以赋值给any，但是any类型失去了类型检测功能，可能导致使用一个不存在的方法和属性也不会报错。
never：never表示不存在的类型
void：没有任何类型
unknown：所有类型都可以赋值给unknown类型，但是unknown类型只能赋值给unknown类型和any类型，他和any相似，但是他不会失去类型检测，一般使用unknown代替any
null & undefined：null和undefined是所有类型的子类型，

##  TypeScript 中可以使用 String、Number、Boolean、Symbol、Object 等给类型做声明吗？
```ts
/* 可以 */
let name: string = "bob";
let decLiteral: number = 6;
let isDone: boolean = false;
let sym: symbol = Symbol();
interface Person {
 name: string;
 age: number;
}
```

## TypeScript 中的 this 和 JavaScript 中的 this 有什么差异？
1. `noImplicitThis: true`的情况下，必须去声明this的类型，才能在函数或对象中使用this。
2. ts中箭头函数的this和ES6中的一样。

## TypeScript 中使用 Union Types （联合类型）时有哪些注意事项？
当不确定联合类型到底是哪个类型的时候就只能访问联合类型的所有类型。

## TypeScript 如何设计 Class 的声明？
```ts
class Greeter {
   greeting: string;
   constructor(message: string) {
       this.greeting = message;
   }
   greet(): string{
       return "Hello, " + this.greeting;
   }
}
let greeter = new Greeter("world");
// 在声明类的时候，一般类中都会包含，构造函数、对构造函数中的属性进行类型声明、类中的方法。
```

## TypeScript 中如何联合枚举类型的 Key?
```ts
enum str {
   A,
   B,
   C
}
type strUnion =  keyof typeof str; // 'A' | 'B' | 'C'
```

## TypeScript 中 ?.、??、!、!.、_、** 等符号的含义？
?. 可链用 遇到null或undefined立即停止表达式的执行
?? 空值合并运算符 当左边的值为null或undefined时就返回右边的值，否则返回左边的值
_ 数字分割符 分隔符不会改变数值字面量的值，使人更容易读懂数字 1_101_324，不允许多个分隔符连用
!.加载变量后面，也是排除undefined和null类型
!在变量前表示取反，在变量后表示使null和undefined类型可以赋值给其他类型，并编译通过
** 求幂

## 简单介绍一下 TypeScript 模块的加载机制？
假设有一个导入语句 import { a } from "moduleA";
 1. 首先，编译器会尝试定位需要导入的模块文件，通过绝对或者相对的路径查找方式；
 2. 如果上面的解析失败了，没有查找到对应的模块，编译器会尝试定位一个外部模块声明（.d.ts）；
 3. 最后，如果编译器还是不能解析这个模块，则会抛出一个错误 error TS2307: Cannot find module 'moduleA'.

## 如何使 TypeScript 项目引入并识别编译为 JavaScript 的 npm 库包？
1. 安装对应的ts包，npm install @type/包名 --save
2. 如果没有对应的ts包，那就在编写同名的.d.ts文件

## TypeScript 的 tsconfig.json 中有哪些配置项信息？

## TypeScript 中如何设置模块导入的路径别名？

## 对 TypeScript 类中成员的 public、private、protected、readonly 修饰符的理解？
public: 成员都默认为public，被此限定符修饰的成员是可以被外部访问；
private: 被此限定符修饰的成员是只可以被类的内部访问；
protected: 被此限定符修饰的成员是只可以被类的内部以及类的子类访问;
readonly: 关键字将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。

## keyof 和 typeof 关键字的作用？
typeof是获取变量的类型
keyof是获取索引类型的属性名，并构造成联合类型

## 数组定义的两种方式
1. Array<string>
2. string[]

## TypeScript 的内置数据类型有哪些？
boolean string undefined null number void never any unknown
array tuple（元组）object enum（枚举）

## any与unknown的区别
any类型可以赋值给任何类型，任何类型也可以赋值给any类型。任何类型可以赋值给unknown类型，但是unknown类型只能赋值给unknown类型和any类型。
unknown类型相比any类型更加的严格。any类型完全失去了类型检测能力，但是unknown在大多数操作之前仍然会进行类型检测。设置any类型，如果这个值是undefined，调用他上面的属性也不会报错，但是unknown会报错，因为unknown类型属于未知类型，所以不允许访问属性值。

## 使用ts实现一个判断入参是否是数组类型的方法？
```ts
function isArray(value:unknown):boolean{
  if(Array.isArray(value)){
    return true;
  }
  return false;
}
```

## 如何在TypeScript中实现继承？
extends关键字

## 接口和类型别名
接口和类型别名都可以用来定义对象，函数，类的类型，但是类型别名的话可以定义其他类型，比如，基本类型，元组，联合类型。
类型别名名称一样会报错，但是接口名称一样，如果合法会产生接口合并。
接口使用extends继承，类型别名使用交叉类型继承。接口继承如果有同样类型和名称的参数会报错，类型别名有同样的参数和类型会返回never。
type结合泛型和extends infer可以写一下工具类型

## Typescript中never 和 void 的区别？
never表示永远不存在的类型，void表示没有任何类型。
返回值为void的函数能够正常运行，返回值为never的函数会报错。
void类型，只能将undefined和null赋值给他。
never是所有类型的子类型，可以把never赋值给任何类型，除了never类型本身，any类型也不能赋值给他自身。
never可以赋值给哪些返回错误的函数或根本无法到达终点的类型。

## 类型断言
类型断言就是，如果你知道一个实体具有比现在类型更确切的类型就可以使用类型断言，通过类型断言就是告诉编译器，“相信我，我知道我在做什么”。
他有两种表现方式，一种是`尖括号`方法，一种是`as`，但是在ts中使用jsx只有as这种类型断言被允许。

## 重载
js是一个动态语言，比如一个函数可能因为出入的类型不同返回不同的类型，这就需要重载，重载的函数在调用的时候会进行正确的类型检查。
它会先尝试第一个重载定义，如果能够匹配就使用这个，所以要把最精确的放到最上面。

## Typescript中泛型是什么？
我感觉泛型更像一种类型变量，泛型的值通常在使用的时候才会设置。泛型可以在interface，type，函数中使用。
函数使用时比如函数的返回值类型与传入值类型有关的时候。
type中使用泛型，可以来写一下工具。
因为泛型没有约束造成了一些问题，比如，因为不确定泛型的具体类型，所以访问泛型类型的属性会报错。可以使用extends来对泛型进行约束。

## 解释一下TypeScript中的枚举。
枚举就是创建一组命名的常量，可以是字符串也可以是数组，默认是从0递增的。
如果你给一个常量命名为数字，那么后面的值就会从这个数字开始递增，但是如果你设置的是字符串，那么下面的值也必须设置，否者报错。
枚举在运行的时候是真实存在的对象，你可以通过枚举值向枚举名泛型映射。
如果枚举只是让代码可读性更好，不需要创建对象那么就使用常量枚举。

## declare关键字
当我们使用第三方库的时候，第三方库可能给window上添加属性，如果我们在ts中直接获取这个元素会出现错误，因为ts编译器不认识这个全局变量，所以就需要declare关键字来声明这个全局变量，从而让ts编译器可以识别这个变量。

## 说说对 TypeScript 中命名空间的理解？模块与命名空间的区别？
在ts1.5版本中，“内部模块”被改成了“命名空间”，“外部模块”被称为“模块”。
模块：
在ts中，任何包含顶级`import`或`export`的文件都会被当作一个模块。如果没有顶级的`import`或`export`则会被当作一个全局可见。
命名空间：
命名空间的定义是使用的namespace关键字，如果想要外部可以访问命名空间中的值那么就需要使用export导出。命名空间的本质就是一个对象。

命名空间和模块一样都包含代码和声明，不同的是模块可以声明它的依赖。
命名空间可以使用`import x = NameSpace.polygons`的方法给命名空间设置别名。
相同名称的命名空间会进行合并。
在ts项目开发过程中一般不使用命名空间，通常在为没有ts的js库，定义.d.ts文件的时候使用。

## 装饰器

##  简单聊聊你对 TypeScript 类型兼容性的理解？

## 协变、逆变、双变和抗变的理解？

## 类型的全局声明和局部声明

## 说一说TypeScript中的类及其特性。

## TypeScript 中的类是什么？你如何定义它们？

## TypeScript 中的 getter/setter 是什么？你如何使用它们？

## TypeScript中的方法重写是什么?

## Typescript什么是方法重载？

## TypeScript 中 interface 可以给 Function / Array / Class（Indexable）做声明吗？

## 用过哪些工具类型