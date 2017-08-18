# JavaScript Style Guide

*用更合理的方式写 JavaScript*

## 原则
  - 关注代码可读性
  

## <a name="variables">变量</a>

  - 使用 `var` 声明每一个变量。
    这样做的好处是增加新变量将变的更加容易，而且不用再担心搞错 `;` 跟 `,`。

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems()
    var goSportsTeam = true
    var dragonball = 'z'
    ```


## <a name="comparison-operators">比较运算符</a>

  - 由于 js 是弱类型，计算时会自动转换，会发生意想不到的问题。优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`。

## <a name="blocks">块</a>

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
    else {
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


## <a name="comments">注释</a>

  - 函数开头应该注释说明函数用途


## <a name="whitespace">空白</a>

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
