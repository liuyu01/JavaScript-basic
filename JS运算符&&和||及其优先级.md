##### JS运算符&&和||及其优先级

###### &&(逻辑与)

```
var a = 1 && 2 && 3; //3
var b = 0 && 1 && 2; //0
var c = 1 && 0 && 2; //0
alert(a)
alert(b)
alert(c)
```

a && b，如果a为true，直接返回b，而不管b为true或者false。如果a为false那么直接返回a。

var a = 1 && 2 && 3; 因为1 && 2,1为真，返回2，2&&3，2为真，返回3。

###### ||(逻辑或)

```
var a = 0 || 1 || 2; //1
var b = 1 || 0 ||3; //1
alert(a)
alert(b)
```

||运算遇到true就返回。例如 a||b，如果a为false，直接返回b，而不管b为true或者false。如果a为true，而不会继续往下执行。

###### &&(逻辑与)和||(逻辑或)混合使用的时候要注意他们的优先级：

###### &&优先级高于||

return a && b || c ,
根据a来判断返回值，a 是 false 则肯定返回 c；如果 b , c 都是 true ，那么我们就可以根据 a 来决定b 还是 c ，如果 a 是 false 则返回 c，如果a是true 则返回 b。
return a || b && c
根据优先级相当于先算 b && c ,然后和a 相 或；如果a是true，则返回a，不论是b或c，如果a是false，则如果b是false，返回b，如果b是true，返回c；

```
var a = 3 && 0 || 2; //2
var b = 3 || 0 && 2; //3
var c = 0 || 2 && 3; //3
alert(a)
alert(b)
alert(c)
```

