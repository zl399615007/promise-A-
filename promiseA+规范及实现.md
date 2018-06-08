Promise表示一个异步操作的最终结果。与Promise最主要的交互方法是通过将函数传入它的then方法从而获取得Promise最终的值或Promise最终被拒绝（reject）的原因。

# 1.术语
```promise``` 是一个包含了兼容promise规范then方法的对象或函数,  
```thenable``` 是一个包含了then方法的对象或函数。  
```value``` 是任何Javascript值。 (包括 undefined, thenable, promise等)。
```exception```是由throw表达式抛出来的值。  
```reason```  是一个用于描述Promise被拒绝原因的值。
# 2.要求
## 2.1 Promise状态
一个Promise必须处在其中之一的状态：pending, fulfilled 或 rejected.
- 如果是pending状态,则promise：  
  1.可以转换到fulfilled或rejected状态。
- 如果是fulfilled状态,则promise：
  1. 不能转换成任何其它状态。
  2. 必须有一个值，且这个值不能被改变。
- 如果是rejected状态,则promise可以：
  1. 不能转换成任何其它状态。
  2. 必须有一个原因，且这个值不能被改变。
 ## 2.2 ```then```方法
一个Promise必须提供一个then方法来获取其值或原因。
Promise的then方法接受两个参数：  

```promise.then(onFulfilled,onRejected)```
1. ```onFulfilled```和```onRejected```都是可选参数，如果```onFulfilled```或者```onRejected```不是一个函数，则忽略。
2. 如果```onFulfilled```是一个函数：1.它必须在```promise```fulfilled后调用，且```promise```的value为其第一个参数。2.它不能被多次调用
3. 如果```onRejected```是一个函数：1.它必须在```promise``` rejected后调用， 且```promise```的reason为其第一个参数。2.不能被多次调用。
4. 都只允许在ececution context栈仅包含平台代码时运行
5. 都必须当做函数调用
6. 对于一个```promise```，它的then方法可以调用多次。所有的```onFulfilled```或者```onRejected```都必须按照其注册顺序执行。
7. ```then```必须返回一个promise
