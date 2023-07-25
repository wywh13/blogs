# JSON

## 1. JSON的概念

​    JSON是一种是一种轻量级的数据交换格式，它采用完全独立于编程语言的文本格式来存储和表示数据，层次机构简洁清晰，易于人阅读和编写，同时也易于机器解析和生成。这些优点使得 JSON 成为理想的数据交换语言，应用十分广泛。

​    JSON并不是一种新的文件类型，而是一种约定好的用于存储和表示数据的文本格式，本质上是将所要存储的信息按照格式转化而成的文本文件，以此来达成不同编程语言之间的信息传递(比如JavaScript对象-->JSON格式的文本文件-->Java的String类型)。

## 2. JSON语法规则

​    JSON值可以是对象、数组、数字、字符串或者三个字面值(false、null、true)中的一个，其中字面值中的英文必须使用小写，对象由花括号括起来的逗号分割的成员构成，每个成员为用冒号分割的键值对，数组由方括号括起来的一组值组成。

```json
3.14                                     //值
"helloworld!"                            //字符串
{"a" : 1 , "b" : "helloworld!" , "c" : [1, 2, 3]}    //对象
[1, 2.5, "3.14", {"a": 4}]               //数组
false                                    //字面值
```

## 3. JSON与JS对象的转化

​    JSON一开始就是作为JavaScript对象的文本表示法被发明的，现在也大多用于JavaScript对象的存储与传递，因此可以将JSON简单理解为JS对象的字符串表示法，它使用文本表示一个JS对象的信息，本质上是一个字符串。

```json
var obj = {a: 'Hello', b: 'World'};                   //这是一个JavaScript对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}';            //这是一个JSON对象，本质是一个字符串
var obj = JSON.parse('{"a": "Hello", "b": "World"}'); //从JSON字符串转换为JS对象,结果是 {a: 'Hello', b: 'World'}
var json = JSON.stringify({a: 'Hello', b: 'World'});  //从JS对象转换为JSON字符串,结果是 '{"a": "Hello", "b": "World"}'
```

## 