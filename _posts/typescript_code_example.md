```typescript
for (let i: number = 0; i < 5; ++i) {
  let line: string = "";
  for(let j: number = 0; j <= i; ++j) {
    line += "*";
  }
  console.log(line);
}

let isDone: boolean = false;

// error TS2322: Type 'Boolean' is not assignableto type 'boolean'.
//  'boolean' is a primitive, but 'Boolean' is a wrapper object.Prefer using 'boolean' when possible.
// let createdByNewBoolean: boolean = new Boolean(1);

let createdByNewBoolean: Boolean = new Boolean(1);
let createdByBoolean: boolean = Boolean(1);
```


//
//    数值
//

let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;


//
//    字符串
//


// 模板字符串
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
console.log(sentence);

// 用 void 表示没有任何返回值的函数：
function alertName(): void {
    alert('My name is Tom');
}


//
//    Null 和 Undefined
//
// 

// undefined 类型的变量只能被赋值为 undefined，null 类型的变量只能被赋值为 null

// undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量

let num11: number = 3;

// let u: undefined;
// let num2: number = u;



//
//    任意值类型
//
// 
console.log('73');
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;

// 在任意值上访问任何属性都是允许的：
// 允许调用任何方法：

let anyThing: any = 'hello';
// console.log(anyThing.myName);
// console.log(anyThing.myName.firstName);

let anyThing1: any = 'Tom';
// anyThing1.setName('Jerry');
// anyThing1.setName('Jerry').sayHello();
// anyThing1.myName.setFirstName('Cat');
console.log('88');



//
//    联合类型（Union Types）
//
//
let myFavoriteNumber1: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
console.log(typeof(myFavoriteNumber))

// 当TypeScript不确定一个联合类型的变量到底是哪个类型的时候, 我们只能访问此联合类型的所有类型里共有的属性或方法：;
//



//
//    接口（Interfaces）
//
//
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};


// 可选属性
interface Person1 {
    name: string;
    age?: number;           // ? before semi-colon
}
let tom1: Person1 = {
    name: 'Tom'
};

// 任意属性
// 一个接口允许有任意的属性
interface Person2 {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom2: Person2 = {
    name: 'Tom',
    gender: 'male'
};



// 只读属性
// 字段只能在创建的时候被赋值
interface Person3 {
    readonly id: number;      // readonly before name
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom3: Person3 = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};



//
//    数组的类型
//
//
let fibonacci: number[] = [1, 1, 2, 3, 5];
// 数组的项中不允许出现其他的类型：

// 用接口表示数组
interface NumberArray {
    [index: number]: number;
}
let fibonacci1: NumberArray = [1, 1, 2, 3, 5];

// 用 any 表示数组中允许出现任意类型
let list: any[] = ['Xcat Liu', 25, { website: 'http://xcatliu.com' }];


//
//    函数的类型
//
//

function sum(x: number, y: number): number {
    return x + y;
}


// 用接口定义函数的形状
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}

// 可选参数
// 可选参数必须接在必需参数后面。换句话说，可选参数后面不允许再出现必须参数了
function buildName(firstName: string, lastName?: string) {    // lastName
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom4 = buildName('Tom');

// 参数默认值
// TypeScript 会将添加了默认值的参数识别为可选参数：
// 此时就不受「可选参数必须接在必需参数后面」的限制了：
function buildName1(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat1 = buildName1('Tom', 'Cat');
let cat = buildName1(undefined, 'Cat');


// 剩余参数
// 数组的类型来定义它
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a11 :any[] = [];
push(a11, 1, 2, 3);


// 重载
// 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
// 输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。
// TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
    return 0;
}


//
//    类型别名
//
//
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}



//
//    字符串字面量类型用来约束取值只能是某几个字符串中的一个
//
//
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}


//
//    元组
//
//
let xcatliu: [string, number] = ['Xcat Liu', 25];

// let xcatliu: [string, number];
xcatliu[0] = 'Xcat Liu';
xcatliu[1] = 25;

xcatliu[0].slice(1);
xcatliu[1].toFixed(2);


//
//    枚举
//
//
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

enum Days1 {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days1["Sun"] === 7); // true
console.log(Days1["Mon"] === 1); // true
console.log(Days1["Tue"] === 2); // true
console.log(Days1["Sat"] === 6); // true


//
//    泛型
//
//
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']

//--------------------------

function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]

//-------------------------- 对泛型进行约束

interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

//------------

function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = (<T>source)[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });


//------------   使用含有泛型的接口来定义函数的形状

interface CreateArrayFunc {
    <T>(length: number, value: T): Array<T>;
}

let createArray1: CreateArrayFunc;
createArray1 = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']



























