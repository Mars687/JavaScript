# JavaScript learning note

### call() 方法与apply() 方法的区别

call() 方法调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。

>注意：该方法的作用和 apply() 方法类似，只有一个区别，就是call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组。

1. 每个函数都包含两个非继承而来的方法：call()方法和apply()方法。

2. 相同点：这两个方法的作用是一样的。

都是在特定的作用域中调用函数，等于设置函数体内this对象的值，以扩充函数赖以运行的作用域。

一般来说，this总是指向调用某个方法的对象，但是使用call()和apply()方法时，就会改变this的指向。

call()方法使用示例：
``` Javascript
    //例1
    <script>
        window.color = 'red';
        document.color = 'yellow';

        var s1 = {color: 'blue' };
        function changeColor(){
            console.log(this.color);
        }

        changeColor.call();         //red (默认传递参数)
        changeColor.call(window);   //red
        changeColor.call(document); //yellow
        changeColor.call(this);     //red
        changeColor.call(s1);       //blue

    </script>

    //例2
    var Pet = {
        words : '...',
        speak : function (say) {
            console.log(say + ''+ this.words)
        }
    }
    Pet.speak('Speak'); // 结果：Speak...

    var Dog = {
        words:'Wang'
    }

    //将this的指向改变成了Dog
    Pet.speak.call(Dog, 'Speak'); //结果： SpeakWang
```

apply()方法使用示例：

``` Javascript
    //例1
    <script>
        window.number = 'one';
        document.number = 'two';

        var s1 = {number: 'three' };
        function changeColor(){
            console.log(this.number);
        }

        changeColor.apply();         //one (默认传参)
        changeColor.apply(window);   //one
        changeColor.apply(document); //two
        changeColor.apply(this);     //one
        changeColor.apply(s1);       //three

    </script>

    //例2
    function Pet(words){
        this.words = words;
        this.speak = function () {
            console.log( this.words)
        }
    }
    function Dog(words){
        //Pet.call(this, words); //结果： Wang
       Pet.apply(this, arguments); //结果： Wang
    }
    var dog = new Dog('Wang');
    dog.speak();
```

3. 不同点：接收参数的方式不同。

apply()方法 接收两个参数，一个是函数运行的作用域（this），另一个是参数数组。
语法：apply([thisObj [,argArray] ]);，调用一个对象的一个方法，2另一个对象替换当前对象。

说明：如果argArray不是一个有效数组或不是arguments对象，那么将导致一个 
TypeError，如果没有提供argArray和thisObj任何一个参数，那么Global对象将用作thisObj。

call()方法 第一个参数和apply()方法的一样，但是传递给函数的参数必须列举出来。
语法：call([thisObject[,arg1 [,arg2 [,...,argn]]]]);，应用某一对象的一个方法，用另一个对象替换当前对象。

说明： call方法可以用来代替另一个对象调用一个方法，call方法可以将一个函数的对象上下文从初始的上下文改变为thisObj指定的新对象，如果没有提供thisObj参数，那么Global对象被用于thisObj。

使用示例1：
``` Javascript
    function add(c,d){
        return this.a + this.b + c + d;
    }

    var s = {a:1, b:2};
    console.log(add.call(s,3,4)); // 1+2+3+4 = 10
    console.log(add.apply(s,[5,6])); // 1+2+5+6 = 14 
```

使用示例2：
``` Javascript
    <script>
        window.firstName = "Cynthia"; 
        window.lastName = "_xie";

        var myObject = {firstName:'my', lastName:'Object'};

        function getName(){
            console.log(this.firstName + this.lastName);
        }

        function getMessage(sex,age){
            console.log(this.firstName + this.lastName + " 性别: " + sex + " age: " + age );
        }

        getName.call(window); // Cynthia_xie
        getName.call(myObject); // myObject

        getName.apply(window); // Cynthia_xie
        getName.apply(myObject);// myObject

        getMessage.call(window,"女",21); //Cynthia_xie 性别: 女 age: 21
        getMessage.apply(window,["女",21]); // Cynthia_xie 性别: 女 age: 21

        getMessage.call(myObject,"未知",22); //myObject 性别: 未知 age: 22
        getMessage.apply(myObject,["未知",22]); // myObject 性别: 未知 age: 22

    </script>
```

### KeyPress 和KeyDown 、KeyPress之间的区别

系统由KeyDown返回键盘的代码, 然后由TranslateMessage函数翻译成成字符, 由KeyPress返回字符值. 因此在KeyDown中返回的是键盘的代码, 而KeyPress返回的是ASCII字符. 所以根据你的目的, 如果只想读取字符, 用KeyPress, 如果想读各键的状态, 用KeyDown. 

keydown：用户在键盘上按下某按键是发生。一直按着某按键则会不断触发（opera浏览器除外）。
keypress：用户按下一个按键，并产生一个字符时发生（也就是不管类似shift、alt、ctrl之类的键，就是说用户按了一个能在屏幕上输出字符的按键keypress事件才会触发）。一直按着某按键则会不断触发。
keyup：用户释放某一个按键是触发。

### 一个函数时加上()与不加（）的区别
函数只要是要调用它进行执行的，都必须加括号。此时，函数实际上等于函数的返回值或者执行效果，当然，有些没有返回值，但已经执行了函数体内的行为，就是说，加括号的，就代表将会执行函数体代码。
不加括号的，都是把函数名称作为函数的指针，一个函数的名称就是这个函数的指针，此时不是得到函数的结果，因为不会运行函数体代码。它只是传递了函数体所在的地址位置，在需要的时候好找到函数体去执行。
 
例如window.onload=init;
init函数并不会在这行代码时就执行，浏览器加载文档时这句话会被加载，会被告知文档加载完要执行哪个函数，但实际上没有当时就执行，等到整个文档加载完成之后才会通过init这个指针去执行init()。
 
所以一般时候我们都是采用的是无括号的原因。这也是由于括号的二义性，因为括号是“函数调用运算符”，相当于在执行这样一个函数,所以产生的问题在理解了之后也就理解了。


### 按位非运算符“~”

先看看w3c的定义：

位运算 NOT 由否定号（~）表示，它是 ECMAScript 中为数不多的与二进制算术有关的运算符之一。

位运算 NOT 是三步的处理过程：

* 把运算数转换成 32 位数字

* 把二进制数转换成它的二进制反码（0->1, 1->0）

* 把二进制数转换成浮点数

简单的理解，对任一数值 x 进行按位非操作的结果为 -(x + 1)

``` javascript
console.log('~null: ', ~null);       // => -1
console.log('~undefined: ', ~undefined);  // => -1
console.log('~0: ', ~0);          // => -1
console.log('~{}: ', ~{});         // => -1
console.log('~[]: ', ~[]);         // => -1
console.log('~(1/0): ', ~(1/0));      // => -1
console.log('~false: ', ~false);      // => -1
console.log('~true: ', ~true);       // => -2
console.log('~1.2543: ', ~1.2543);     // => -2
console.log('~4.9: ', ~4.9);       // => -5
console.log('~(-2.999): ', ~(-2.999));   // => 1
```

那么, ~~x 就为 -(-(x+1) + 1)

``` javascript
console.log('~~null: ', ~~null);       // => 0
console.log('~~undefined: ', ~~undefined);  // => 0
console.log('~~0: ', ~~0);          // => 0
console.log('~~{}: ', ~~{});         // => 0
console.log('~~[]: ', ~~[]);         // => 0
console.log('~~(1/0): ', ~~(1/0));      // => 0
console.log('~~false: ', ~~false);      // => 0
console.log('~~true: ', ~~true);       // => 1
console.log('~~1.2543: ', ~~1.2543);     // => 1
console.log('~~4.9: ', ~~4.9);       // => 4
console.log('~~(-2.999): ', ~~(-2.999));   // => -2
```

#### ~value的使用
判断数值中是否有某元素时，以前这样判断：

>     if(arr.indexOf(ele) > -1){...} //易读
现在可以这样判断，两者效率：

>     if(~arr.indexOf(ele)){...} //简洁

#### ~~value的使用

对于浮点数，~~value可以代替parseInt(value)，而且前者效率更高些

>     parseInt(-2.99) //-2
>     ~~(-2.99) //-2

测试

``` javascript
var time1 = +new Date();
var count = 5000000;
var ele = 1;
var arr = [1,2,4,5,2];
var h = 1.01;

console.time('parseInt');
for (var i = count; i > 0; i--) {
    parseInt(h);
}
console.timeEnd('parseInt'); //84.385ms

console.time('~~');
for (var i = count; i>0; i--) {
    ~~h;
}
console.timeEnd('~~'); //13.386ms


console.time('arr.indexOf(ele) > -1');
for (var j = count; j>0; j--) {
    arr.indexOf(ele) > -1;
}
console.timeEnd('arr.indexOf(ele) > -1'); //16.263ms

console.time('~arr.indexOf(ele)');
for (var i = count; i>0; i--) {
    ~arr.indexOf(ele);
}
```
