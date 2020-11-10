# js-block-parser

Coarse-grained JavaScript block parser.

### 动机

svelte 类似的框架中，对 js 的编译是使用了acorn 这种轻量级的 js parser

但是要知道，svelte 对 js 语义的改变是不多的，是不需要标准的 AST 结构的，虽然一般来说正则就足够了……

但是为了代码质量，我们仍旧是需要生成 AST，只不过不需要很细的粒度

比如下面这段代码

```js
let count = 0
function add() {
  count++
}
```
使用 acorn 会生成这样的结构：

<details>
<summary>太长了</summary>

```json
{
  "type": "Program",
  "start": 0,
  "end": 42,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 13,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 4,
          "end": 13,
          "id": {
            "type": "Identifier",
            "start": 4,
            "end": 9,
            "name": "count"
          },
          "init": {
            "type": "Literal",
            "start": 12,
            "end": 13,
            "value": 0,
            "raw": "0"
          }
        }
      ],
      "kind": "let"
    },
    {
      "type": "FunctionDeclaration",
      "start": 14,
      "end": 42,
      "id": {
        "type": "Identifier",
        "start": 23,
        "end": 26,
        "name": "add"
      },
      "expression": false,
      "generator": false,
      "async": false,
      "params": [],
      "body": {
        "type": "BlockStatement",
        "start": 29,
        "end": 42,
        "body": [
          {
            "type": "ExpressionStatement",
            "start": 33,
            "end": 40,
            "expression": {
              "type": "UpdateExpression",
              "start": 33,
              "end": 40,
              "operator": "++",
              "prefix": false,
              "argument": {
                "type": "Identifier",
                "start": 33,
                "end": 38,
                "name": "count"
              }
            }
          }
        ]
      }
    }
  ],
  "sourceType": "module"
}
```
</details>

我们需要简化这个结构，

```js
bdoy: [
  { type:'declaration', identifier: 'count', value: '0'},
  { type: 'function', identifier: 'add', args: '', body: [
    { type: 'expression', identifier: 'count', operator: '++'}
  ]}
]
```
简化 AST 结构，使用我们最常见的树的格式……

实际上按照我的经验，任何 AST 都可以使用类似的结构，无论是 html-parser 还是 jsx（vdom）






