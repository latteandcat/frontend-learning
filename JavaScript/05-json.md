# JSON

## 认识 JSON

- JSON 是一种非常重要的数据格式，它并不是编程语言，而是一种可以在服务器和客户端之间传输的数据格式
- JSON 的全称是 JavaScript Object Notation（JavaScript对象符号）
- 很多编程语言都实现了将 JSON 转成对应模型的方式
- 其他传输格式
  - XML
  - Protobuf

- JSON 的使用场景
  - 网络数据的传输
  - 项目的某些配置文件
  - 非关系型数据库（NoSQL）将json作为存储格式

## JSON 语法

### JSON 值类型

JSON的顶层支持三种类型的值

- 简单值：数字（Number）、字符串（String，不支持单引号）、布尔类型（Boolean）、null类型
- 对象值：由 `"key": value` 组成，key 是字符串类型，并且必须添加双引号，value 可以是简单值、对象值、数组值
- 数组值：数组的值可以是简单值、对象值、数组值

### JSON 序列化

JSON 序列化就是把复杂类型转化成 JSON 格式的字符串，方便对其进行处理

比如对象形式直接存储到 localStorage 中时会被转化为 [object object] 格式的字符串

需要先将其序列化为对应的 JSON 字符串再进行存储，使用时再取出来进行反序列化

- 序列化：`JSON.stringify(value[, replacer [, space]])` 

  将 JavaScript 对象或值转成对应的 JSON 字符串

  - value：将要序列化成一个 JSON 字符串的值

  - replacer（可选）

    - 如果提供一个函数：则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理

      ```js
      var result = JSON.stringify(obj, function(key, value) {
      	if (key === "name") {
              return "replaceName"
          }
          return value
      })
      ```

    - 如果提供一个数组：则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中

    - 为 null 或未提供：则对象所有的属性都会被序列化

  - space（可选）：指定缩进用的空白字符串，用于美化输出（pretty-print）

  注意：转换值如果有 toJSON 方法，那么会直接使用 toJSON 方法的结果

- 反序列化：`JSON.parse(text[, reviver])` 

  解析 JSON 字符串，转回对应的 JavaScript 类型

  - text：要被解析成 JavaScript 值的字符串

  - reviver：转换器，如果传入该参数 (函数)，可以用来修改解析生成的原始值，调用时机在 parse 函数返回之前

    ```js
    JSON.parse('{"p": 5}' , function (k, v) {
      if (k === "") return v; // 如果到了最顶层，则直接返回属性值，
      return v * 2; // 否则将属性值变为原来的 2 倍。
    }); // { p: 10 }
    ```

    