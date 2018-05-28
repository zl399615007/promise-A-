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
  
