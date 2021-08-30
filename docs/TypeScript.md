# TypeScript 手册

## TypeScript 介绍

- TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。
- TypeScript 是面向对象的 JavaScript。

- ypeScript 由微软开发的自由和开源的编程语言。

- TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

- TypeScript 是一种给 JavaScript 添加特性的语言扩展。
- TypeScript 通过类型注解提供编译时的静态类型检查。
- TypeScript 遵循强类型，只能赋值指定的类型，如果赋值其它类型就会报错。



在定义变量时，请尽量声明类型或定义接口。



## TypeScript 语法构成

- 模块

- 函数

- 变量

- 语句与表达式

- 注释



## TypeScript 基础类型

任意类型    ----    any

数字类型    ----    number

字符串类型    ----    string

布尔类型    ----    boolean

枚举类型    ----    enum

无返回值    ----    void

对象值缺失    ----    null

未定义    ----    undefined

从不出现的值    ----    never





## 类型断言 

语法: <类型>值 或 值 as 类型

```ts
interface ITypes {
    x: string,
    y: number
}
const itypes = { x: 'hello', y: 120 } as ITypes
```





## 类型推断

当类型没有给出时，TypeScript 编译器利用类型推断来推断类型。由于缺乏声明而不能推断出类型，那么它的类型被视作默认的动态 any 类型。



## 变量作用域

作用域: 决定一个变量的使用范围。

- 全局作用域: 定义在程序结构外部,可在任意地方使用。

- 类作用域: 定义在类内部的属性或方法，只能在类内部使用。

- 局部作用域: 定义在方法的变量，只能做方法内部使用。



## 运算符

同 JavaScript。



## 条件语句

同 JavaScript。



## 循环语句

同 JavaScript。



## 函数

同 JavaScript。

返回值最好给定值类型,无返回值请用 void。



## 包装对象

### Number

```ts
const num = new Number(12);
```



#### 方法

##### 计数法    ----    toExponential()

##### 小数点    ----    toFixed(num)

##### 本地数字格式化    ----    toLocaleString()

##### 数字格式化指定长度    ----    toPrecision(num)

##### 转为字符串(进制)    ----    toString(base)





### String

```ts
const str = new String('132456');
```



#### 方法

##### 返回指定位置字符    ----    charAt(num)

##### 返回指定位置字符的 Unicode 码    ----    charCodeAt(num)

##### 连接多个字符串并返回新字符串    ----    concat(str1, str2, ...)

##### 查找指定字符首次出现位置    ----    indexOf(str)

##### 比较字符串是否一致    ----    localeCompare(str2)

##### 查找到一个或多个正则匹配项    ----    match(reg)

##### 替代正则匹配项子串    ----    replace(reg)

##### 检索与正则匹配的值    ----    search(reg)

##### 返回提取的字符串片段    ----    slice(s, e)

##### 分割字符串并返回指定个数    ----    split(s, n)

##### 返回检索位置起并指定个数的字符    ----    substr(s, l)

##### 返回检索起始位置之间的字符    ----    substring(s, e)

##### 返回小写字符串    ----    toLowerCase()

##### 返回大写字符串    ----    toUpperCase()





## 数组

TypeScript 中的数组，是指一系列相同类型值的数组。

```ts
const arr1 = new Array('a', 'b', 'c');
const arr2: number = [1, 2, 3];
```



### 方法

##### 返回多个合并后的新数组    ----    concat(arr2, arr3, ...)

检测每一数组项是否匹配条件    ----    every((item, index, array) => { ... })

返回所有匹配项     ----    filter((item, index, array) => { ... })

检测数组项是否有匹配条件    ----    some((item, index, array) => { ... })

对每一数组项进行回调处理    ----    forEach((item, index, array) => { ... })

返回指定数组项的首次位置    ----    indexOf(item)

返回连接后的字符串    ----    join(s)

对每一数组项进行处理    ----    map((item, index, array) => { ... })

删除并返回数组最后一项    ----    pop()

向数组末尾添加多个元素    ----    push(item1, item2, ...)

对数组进行归并处理    ----    reduce((p, n, index, array) => {  ... })

翻转数组    ----    reverse()

删除并返回第一个元素    ---- shift()

向数组头部添加多个元素    ----    unshift(item1, item2, ...)

返回提取的数组片段    ----    slice(s, e)

对数组项进行排序    ----    sort()

从数组指定位置添加或删除元素    ----     splice(s, l, item1, item2, ...)

转换为字符串    ----    toString()





## 元组

TypeScript 中的元组，是指一系列不同类型值的数组。

```ts
const arr = [10, 'ager', true];
```



### 方法

同数组方法。





## 联合类型

联合类型时指对变量进行声明多个类型，使用管道符号(|)分割。

```ts
var type: number| string | boolean = 'vera';
type = 12;

var arr: (number[] | string[]) = [123, 465, 456];
arr = ['aer', 'aerg', 'tyrhbgs'];
```





## 接口

接口是一系列抽象方法的声明，是一些方法特征的集合。

### 基本案例

```ts
// 声明抽象方法
interface IAnimal {
	name: string,
    type: string,
    showInfo: () => string
}

// 定义变量、指定变量类型为 IAnimal，并对接口的抽象方法进行实现
const animal: IAnimal = {
    name: 'hotPig',
    type: 'dog',
    showInfo: (): string => { return 'hotPig' }
};
```



### 联合类型与接口

```ts
interface ICat {
    name: string,
    desc: string[] | string | () => string
}

const cat: ICat = {
    name: 'cat',
    desc: 'abc'
}

cat.desc = ['agre', 'ager'];

cat.desc = function () { return 'okkk' }
```



### 接口与数组

接口中我们可以将数组的索引值和元素设置为不同类型，索引值可以是数字或字符串。

```ts
interface namelist { 
   [index:number]:string // index 代表索引
} 
var list2:namelist = ["John",1,"Bran"] // 错误元素 1 不是 string 类型

interface ages { 
   [index:string]:number 
} 
var agelist:ages; 
agelist["John"] = 15   // 正确 
agelist[2] = "nine"   // 错误
```





### 接口继承

接口继承是指通过其它接口所描述的一系列抽象方法来扩展当前接口，**关键字: extends **。

#### 接口单继承

```ts
interface IAnimal {
    type: string,
    isLand: boolean
    name: string
}

interface ICat extends IAnimal {
    run: ()=>string
}

interface IFish extends IAnimal {
    swim: ()=>string
}

const cat = {
    type: 'cat',
    isLand: true,
    name: 'zr_cat',
    run: function ():string { return 'cat running' }
} as ICat

const fish = {
    type: 'fish',
    isLand: false,
    name: 'rd_fish',
    swim: () => { return 'fish swimming' }
} as IFish

console.log(cat.run()); // 'cat running'
console.log(fish.swim()); // 'fish swimming'

```



#### 接口多继承

```ts
interface IFather {
    y: string,
    type: string
}
interface IMother {
    x: string,
}
interface IChild extends IFather, IMother {
    s: string
}

const child = {
    y: 'from father y',
    type: 'from father type',
    x: 'from mother x',
    s: 'self s'
} as IChild
console.log(child)
```



## 类

TypeScript 是面向对象的 JavaScript。类描述了所创建对象的共同属性和方法。类包含字段，构造函数，方法等模块。

```ts
class Animal {
	field: string; // 字段
    // 构造函数
    constructor () {
        
    }
    // 方法
    show ():void {
        console.log('animal class');
    }
}
```



### 类继承

- TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。

- 类继承使用关键字 **extends**，子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。

```ts
class Animal {
    type: string;
    constructor () {
        this.type = 'animal';
    }
    showType ():void {
        console.log(this.type)
    }
}

class Cat extends Animal {
    type: string;
    name: string;
    constructor (name: string) {
        this.type = 'cat';
        this.name = name;
    }
    showName (): void { console.log(this.name) }
}
const cat = new Cat('dr_cat');
cat.showType();
cat.showName();
```



### 访问控制修饰符

静态成员    ----    static (通过类名访问)

公共成员    ----    public (默认, 任何地方都可访问)

私有成员    ----    private (所在类访问)

受保护的成员    ----    protected  (自身或子类访问)



```ts
class Filed {
    static count: number = 0;
    pub: string = 'filed public';
    private pri:string = 'filed private';
    protected pro:string = 'filed protected';
    constructor () {
        
    }
}

const filed = new Filed();
console.log(filed.count);
console.log(filed.pub);
console.log(filed.pro);
console.log(filed.pri);

// 注意: class 类中无法使用 this.__proto__,因为 __proto__ 不存在。

```



### 类和接口

类可以实现接口，将接口所描述的抽象特性用类的字段实现，关键字：implements。

```ts
interface IClass {
    name: string
}

class Classes implements IClass {
    name: string;
    constructor (name: string) {
        this.name = name;
    }
}

const classes = new Classes('xiaoyimi');
console.log(classes.name);
```





## 对象

对象是包含一组键值对的实例。 值可以是标量、函数、数组、对象等。



### 类型模板

在声明对象时，可能后续会有方法增加；但在 TypeScript 中,后续添加的方法可能报错，所以提供一个类型占位符(初始化值)来表示。

```ts
var my = {
    name: 'xiaoyimi',
    age: 25,
    show: function () {}  // 类型模板
}

my.show = function () { console.log(this.name, this.age) };
my.show();
```



### 鸭子类型

无论怎么看都像鸭子，故称鸭子类型；

```ts
// 抽象的鸭子
interface IPoint { 
    x:number 
    y:number 
} 
// p1， p2 都是类似'鸭子'的对象
function addPoints(p1:IPoint,p2:IPoint):IPoint { 
    var x = p1.x + p2.x 
    var y = p1.y + p2.y 
    return {x:x,y:y} // 返回类似'鸭子'的对象
} 
 
// 正确
var newPoint = addPoints({x:3,y:4},{x:5,y:1})  
 
// 错误 
var newPoint2 = addPoints({x:1},{x:4,y:3})
```





## 命名空间

命名空间一个最明确的目的就是**解决重名问题**。





## 模块



## 声明文件



