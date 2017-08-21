# JavaScript Style Guide

*用更合理的方式写 JavaScript*

## 原则
  - 关注代码可读性

## 减少使用的用法
  - `with` 语句
  - `for` 循环多重嵌套
  - 三元表达式
  - 正则表达式

## 数字
    - 不要省略小数点

    ```javascript
    // bad
    var a = .5

    // good
    var a = 0.5
    ```

## 变量

  - 使用 `var` 声明每一个变量。

    ```javascript
    // bad
    // 这里若逗号敲成了分号，会变成全局变量
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // good
    var items = getItems()
    var goSportsTeam = true
    var dragonball = 'z'

    // bad
    var a = b = 0;
    // 相当于
    b = 0
    var a = b
    // b 变成了全局变量
    ```
  - 大段代码中减少使用含糊命名

  ```javascript
  // bad
  var som = 0
  var som1 = 0
  var som2 = 0
  var som3 = 0
  var som4 = 0
  ```

## 函数

  - 单个函数尽量不要超过 50 行
  - 公共函数应该封装在对象中，作为对象的方法调用
  - 减少使用拼音命名

    ```javascript
    // bad
    var paixu
    var fenlei
    // 加载类目
    var addlm = function() {
    //   code
    }
    ```

  - 函数的参数是对象时，若有多个值，用变量传递
    ```javascript
    // bad
    ajax("/limit_discount/ajax_check_status", {'num_iids':processing_num_iids, 'act_id':promotion_id, 'operation':operation}, function(data){
        // code
    })

    // good
    var params = {
        'num_iids': processing_num_iids,
        'act_id': promotion_id,
        'operation': operation,
    }
    ajax("/limit_discount/ajax_check_status", params, function(data){
        // code
    })
    ```

## 比较运算符

  - 由于 js 是弱类型，计算时会自动转换，会发生意想不到的问题。优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`。

## 大括号

  - 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
        return false

    if (test) return false

    // good
    if (test) {
        return false
    }
    ```

  - 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。

    ```javascript
    // bad
    if (test) {
        thing1()
        thing2()
    }
    else
    {
        thing3()
    }

    // good
    if (test) {
        thing1()
        thing2()
    } else {
        thing3()
    }
    ```


## 注释

  - 函数开头应该注释说明函数用途


## 空白与缩进

  - 使用 一个 `tab = 4 个空格` 作为缩进。

    ```javascript
    // bad
    function () {
    ∙∙var name
    }

    // bad
    function () {
    ∙var name
    }

    // good
    function () {
        var name
    }
    ```

  - 使用空格把运算符隔开。

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```
 - 参数列表中逗号后面需要空格。批量处理可以使用格式化插件。
     ```javascript
    // bad
    var filter = function (a,b,c,d) {
        // var name
    }

    // good
    var filter = function (a, b, c, d) {
        // var name
    }
    ```
