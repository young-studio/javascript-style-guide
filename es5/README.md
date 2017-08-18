# Airbnb JavaScript Style Guide() {  

*用更合理的方式写 JavaScript*

## <a name="table-of-contents">目录</a>

  1. [类型](#types)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [函数](#functions)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升](#hoisting)
  1. [比较运算符 & 等号](#comparison-operators--equality)
  1. [块](#blocks)
  1. [注释](#comments)
  1. [空白](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转化](#type-casting--coercion)
  1. [命名规则](#naming-conventions)
  1. [存取器](#accessors)
  1. [构造函数](#constructors)
  1. [事件](#events)
  1. [模块](#modules)
  1. [jQuery](#jquery)

## <a name="types">类型</a>

  - **原始值**: 存取直接作用于它自身。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **复杂类型**: 存取时作用于它自身值的引用。

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="objects">对象</a>

  - 使用直接量创建对象。

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```


**[⬆ 回到顶部](#table-of-contents)**

## <a name="arrays">数组</a>

  - 使用直接量创建数组。

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 向数组增加元素时使用 Array#push 来替代直接赋值。

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```


  - 使用 Array#slice 将类数组对象转换成数组。

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**



## <a name="functions">函数</a>

  - 函数表达式：

    ```javascript
    // 匿名函数表达式
    var anonymous = function() {
      return true;
    };

    // 命名函数表达式
    var named = function named() {
      return true;
    };

    // 立即调用的函数表达式（IIFE）
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```


**[⬆ 回到顶部](#table-of-contents)**



## <a name="variables">变量</a>


  - 使用 `var` 声明每一个变量。
    这样做的好处是增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // （跟上面的代码比较一下，看看哪里错了）
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 在作用域顶部声明变量。这将帮你避免变量声明提升相关的问题。

    ```javascript
    // bad
    function () {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function () {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - 不必要的函数调用
    function () {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function () {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="hoisting">提升</a>

  - 变量声明会提升至作用域顶部，但赋值不会。

    ```javascript
    // 我们知道这样不能正常工作（假设这里没有名为 notDefined 的全局变量）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 但由于变量声明提升的原因，在一个变量引用后再创建它的变量声明将可以正常工作。
    // 注：变量赋值为 `true` 不会提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器会把变量声明提升到作用域顶部，意味着我们的例子将被重写成：
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 匿名函数表达式会提升它们的变量名，但不会提升函数的赋值。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - 命名函数表达式会提升变量名，但不会提升函数名或函数体。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 当函数名跟变量名一样时，表现也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 函数声明提升它们的名字和函数体。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 了解更多信息在 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ 回到顶部](#table-of-contents)**



## <a name="comparison-operators--equality">比较运算符 & 等号</a>

  - 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`.
  - 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则：

    + **对象** 被计算为 **true**
    + **Undefined** 被计算为 **false**
    + **Null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0、-0 或 NaN** 被计算为 **false**，否则为 **true**
    + **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

    ```javascript
    if ([0]) {
      // true
      // 一个数组就是一个对象，对象被计算为 true
    }
    ```

  - 使用快捷方式。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 了解更多信息在 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ 回到顶部](#table-of-contents)**


## <a name="blocks">块</a>

  - 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function () { return false; }

    // good
    function () {
      return false;
    }
    ```

  - 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ 回到顶部](#table-of-contents)**


## <a name="comments">注释</a>

  - 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }
    ```

  - 给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  - 使用 `// FIXME:` 标注问题。

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - 使用 `// TODO:` 标注问题的解决方式。

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="whitespace">空白</a>

  - 使用 2 个空格作为缩进。

    ```javascript
    // bad
    function () {
    ∙∙∙∙var name;
    }

    // bad
    function () {
    ∙var name;
    }

    // good
    function () {
    ∙∙var name;
    }
    ```

  - 在大括号前放一个空格。

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - 使用空格把运算符隔开。

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - 在文件末尾插入一个空行。

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

**[⬆ 回到顶部](#table-of-contents)**



## <a name="type-casting--coercion">类型转换</a>

  - 在语句开始时执行类型转换。
  - 字符串：

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```


  - 布尔:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="naming-conventions">命名规则</a>

  - 避免单字母命名。命名应具备描述性。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - 使用驼峰式命名对象、函数和实例。

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 使用帕斯卡式命名构造函数或类。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - 私有变量使用下划线前缀。

**[⬆ 回到顶部](#table-of-contents)**

