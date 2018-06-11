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

<html>
       <div data-note-content class="show-content">
          <div class="show-content-free">
            <h3>1.什么是Promise?</h3>
<blockquote>
<p>Promise是JS异步编程中的重要概念，异步抽象处理对象，是目前比较流行Javascript异步编程解决方案之一</p>
</blockquote>
<h3>2.对于几种常见异步编程方案</h3>
<ul>
<li>回调函数</li>
<li>事件监听</li>
<li>发布/订阅</li>
<li>Promise对象</li>
</ul>
<h4>这里就拿回调函数说说</h4>
<p>1.对于回调函数 我们用Jquery的ajax获取数据时 都是以回调函数方式获取的数据</p>
<pre><code>$.get(url, (data) =&gt; {
    console.log(data)
)
</code></pre>
<p>2.如果说 当我们需要发送多个异步请求 并且每个请求之间需要相互依赖 那这时 我们只能 以嵌套方式来解决 形成 "回调地狱"</p>
<pre><code>$.get(url, data1 =&gt; {
    console.log(data1)
    $.get(data1.url, data2 =&gt; {
        console.log(data1)
    })
})
</code></pre>
<blockquote>
<p>这样一来，在处理越多的异步逻辑时，就需要越深的回调嵌套，这种编码模式的问题主要有以下几个：</p>
</blockquote>
<ul>
<li>代码逻辑书写顺序与执行顺序不一致，不利于阅读与维护。</li>
<li>异步操作的顺序变更时，需要大规模的代码重构。</li>
<li>回调函数基本都是匿名函数，bug 追踪困难。</li>
<li>回调函数是被第三方库代码（如上例中的 ajax ）而非自己的业务代码所调用的，造成了 IoC 控制反转。</li>
</ul>
<h4>Promise 处理多个相互关联的异步请求</h4>
<p>1.而我们Promise 可以更直观的方式 来解决 "回调地狱"</p>
<pre><code>const request = url =&gt; { 
    return new Promise((resolve, reject) =&gt; {
        $.get(url, data =&gt; {
            resolve(data)
        });
    })
};

// 请求data1
request(url).then(data1 =&gt; {
    return request(data1.url);   
}).then(data2 =&gt; {
    return request(data2.url);
}).then(data3 =&gt; {
    console.log(data3);
}).catch(err =&gt; throw new Error(err));
</code></pre>
<p>2.相信大家在 vue/react 都是用axios fetch 请求数据 也都支持 Promise API</p>
<pre><code>import axios from 'axios';
axios.get(url).then(data =&gt; {
   console.log(data)
})
</code></pre>
<blockquote>
<p>Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。</p>
</blockquote>
<h3>3.Promise使用</h3>
<h4>1.Promise 是一个构造函数， new Promise 返回一个 promise对象 接收一个excutor执行函数作为参数, excutor有两个函数类型形参resolve reject</h4>
<pre><code>const promise = new Promise((resolve, reject) =&gt; {
       // 异步处理
       // 处理结束后、调用resolve 或 reject
});

</code></pre>
<h4>2.promise相当于一个状态机</h4>
<p>promise的三种状态</p>
<ul>
<li>pending</li>
<li>fulfilled</li>
<li>rejected</li>
</ul>
<p>1.promise 对象初始化状态为 pending<br>
2.当调用resolve(成功)，会由pending =&gt; fulfilled<br>
3.当调用reject(失败)，会由pending =&gt; rejected</p>
<blockquote>
<p>注意promsie状态 只能由 pending =&gt; fulfilled/rejected, 一旦修改就不能再变</p>
</blockquote>
<h4>3.promise对象方法</h4>
<p>1.then方法注册 当resolve(成功)/reject(失败)的回调函数</p>
<pre><code>// onFulfilled 是用来接收promise成功的值
// onRejected 是用来接收promise失败的原因
promise.then(onFulfilled, onRejected);
</code></pre>
<blockquote>
<p>then方法是异步执行的</p>
</blockquote>
<p>2.resolve(成功) onFulfilled会被调用</p>
<pre><code>const promise = new Promise((resolve, reject) =&gt; {
   resolve('fulfilled'); // 状态由 pending =&gt; fulfilled
});
promise.then(result =&gt; { // onFulfilled
    console.log(result); // 'fulfilled' 
}, reason =&gt; { // onRejected 不会被调用
    
})
</code></pre>
<p>3.reject(失败) onRejected会被调用</p>
<pre><code>const promise = new Promise((resolve, reject) =&gt; {
   reject('rejected'); // 状态由 pending =&gt; rejected
});
promise.then(result =&gt; { // onFulfilled 不会被调用
  
}, reason =&gt; { // onRejected 
    console.log(rejected); // 'rejected'
})
</code></pre>
<p>4.promise.catch</p>
<blockquote>
<p>在链式写法中可以捕获前面then中发送的异常,</p>
</blockquote>
<pre><code>promise.catch(onRejected)
相当于
promise.then(null, onRrejected);

// 注意
// onRejected 不能捕获当前onFulfilled中的异常
promise.then(onFulfilled, onRrejected); 

// 可以写成：
promise.then(onFulfilled)
       .catch(onRrejected);   
</code></pre>
<h4>4.promise chain</h4>
<blockquote>
<p>promise.then方法每次调用 都返回一个新的promise对象 所以可以链式写法</p>
</blockquote>
<pre><code>function taskA() {
    console.log("Task A");
}
function taskB() {
    console.log("Task B");
}
function onRejected(error) {
    console.log("Catch Error: A or B", error);
}

var promise = Promise.resolve();
promise
    .then(taskA)
    .then(taskB)
    .catch(onRejected) // 捕获前面then方法中的异常
</code></pre>
<h4>5.Promise的静态方法</h4>
<p>1.Promise.resolve 返回一个fulfilled状态的promise对象</p>
<pre><code>Promise.resolve('hello').then(function(value){
    console.log(value);
});

Promise.resolve('hello');
// 相当于
const promise = new Promise(resolve =&gt; {
   resolve('hello');
});
</code></pre>
<p>2.Promise.reject 返回一个rejected状态的promise对象</p>
<pre><code>Promise.reject(24);
new Promise((resolve, reject) =&gt; {
   reject(24);
});
</code></pre>
<p>3.Promise.all 接收一个promise对象数组为参数</p>
<blockquote>
<p>只有全部为resolve才会调用 通常会用来处理 多个并行异步操作</p>
</blockquote>
<pre><code>const p1 = new Promise((resolve, reject) =&gt; {
    resolve(1);
});

const p2 = new Promise((resolve, reject) =&gt; {
    resolve(2);
});

const p3 = new Promise((resolve, reject) =&gt; {
    reject(3);
});

Promise.all([p1, p2, p3]).then(data =&gt; { 
    console.log(data); // [1, 2, 3] 结果顺序和promise实例数组顺序是一致的
}, err =&gt; {
    console.log(err);
});
</code></pre>
<p>4.Promise.race 接收一个promise对象数组为参数</p>
<blockquote>
<p>Promise.race 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。</p>
</blockquote>
<pre><code>function timerPromisefy(delay) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve(delay);
        }, delay);
    });
}
var startDate = Date.now();

Promise.race([
    timerPromisefy(10),
    timerPromisefy(20),
    timerPromisefy(30)
]).then(function (values) {
    console.log(values); // 10
});
</code></pre>
<h3>4. Promise 代码实现</h3>
<pre><code>/**
 * Promise 实现 遵循promise/A+规范
 * Promise/A+规范译文:
 * https://malcolmyu.github.io/2015/06/12/Promises-A-Plus/#note-4
 */

// promise 三个状态
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

function Promise(excutor) {
    let that = this; // 缓存当前promise实例对象
    that.status = PENDING; // 初始状态
    that.value = undefined; // fulfilled状态时 返回的信息
    that.reason = undefined; // rejected状态时 拒绝的原因
    that.onFulfilledCallbacks = []; // 存储fulfilled状态对应的onFulfilled函数
    that.onRejectedCallbacks = []; // 存储rejected状态对应的onRejected函数

    function resolve(value) { // value成功态时接收的终值
        if(value instanceof Promise) {
            return value.then(resolve, reject);
        }

        // 为什么resolve 加setTimeout?
        // 2.2.4规范 onFulfilled 和 onRejected 只允许在 execution context 栈仅包含平台代码时运行.
        // 注1 这里的平台代码指的是引擎、环境以及 promise 的实施代码。实践中要确保 onFulfilled 和 onRejected 方法异步执行，且应该在 then 方法被调用的那一轮事件循环之后的新执行栈中执行。

        setTimeout(() =&gt; {
            // 调用resolve 回调对应onFulfilled函数
            if (that.status === PENDING) {
                // 只能由pedning状态 =&gt; fulfilled状态 (避免调用多次resolve reject)
                that.status = FULFILLED;
                that.value = value;
                that.onFulfilledCallbacks.forEach(cb =&gt; cb(that.value));
            }
        });
    }

    function reject(reason) { // reason失败态时接收的拒因
        setTimeout(() =&gt; {
            // 调用reject 回调对应onRejected函数
            if (that.status === PENDING) {
                // 只能由pedning状态 =&gt; rejected状态 (避免调用多次resolve reject)
                that.status = REJECTED;
                that.reason = reason;
                that.onRejectedCallbacks.forEach(cb =&gt; cb(that.reason));
            }
        });
    }

    // 捕获在excutor执行器中抛出的异常
    // new Promise((resolve, reject) =&gt; {
    //     throw new Error('error in excutor')
    // })
    try {
        excutor(resolve, reject);
    } catch (e) {
        reject(e);
    }
}

/**
 * resolve中的值几种情况：
 * 1.普通值
 * 2.promise对象
 * 3.thenable对象/函数
 */

/**
 * 对resolve 进行改造增强 针对resolve中不同值情况 进行处理
 * @param  {promise} promise2 promise1.then方法返回的新的promise对象
 * @param  {[type]} x         promise1中onFulfilled的返回值
 * @param  {[type]} resolve   promise2的resolve方法
 * @param  {[type]} reject    promise2的reject方法
 */
function resolvePromise(promise2, x, resolve, reject) {
    if (promise2 === x) {  // 如果从onFulfilled中返回的x 就是promise2 就会导致循环引用报错
        return reject(new TypeError('循环引用'));
    }

    let called = false; // 避免多次调用
    // 如果x是一个promise对象 （该判断和下面 判断是不是thenable对象重复 所以可有可无）
    if (x instanceof Promise) { // 获得它的终值 继续resolve
        if (x.status === PENDING) { // 如果为等待态需等待直至 x 被执行或拒绝 并解析y值
            x.then(y =&gt; {
                resolvePromise(promise2, y, resolve, reject);
            }, reason =&gt; {
                reject(reason);
            });
        } else { // 如果 x 已经处于执行态/拒绝态(值已经被解析为普通值)，用相同的值执行传递下去 promise
            x.then(resolve, reject);
        }
        // 如果 x 为对象或者函数
    } else if (x != null &amp;&amp; ((typeof x === 'object') || (typeof x === 'function'))) {
        try { // 是否是thenable对象（具有then方法的对象/函数）
            let then = x.then;
            if (typeof then === 'function') {
                then.call(x, y =&gt; {
                    if(called) return;
                    called = true;
                    resolvePromise(promise2, y, resolve, reject);
                }, reason =&gt; {
                    if(called) return;
                    called = true;
                    reject(reason);
                })
            } else { // 说明是一个普通对象/函数
                resolve(x);
            }
        } catch(e) {
            if(called) return;
            called = true;
            reject(e);
        }
    } else {
        resolve(x);
    }
}

/**
 * [注册fulfilled状态/rejected状态对应的回调函数]
 * @param  {function} onFulfilled fulfilled状态时 执行的函数
 * @param  {function} onRejected  rejected状态时 执行的函数
 * @return {function} newPromsie  返回一个新的promise对象
 */
Promise.prototype.then = function(onFulfilled, onRejected) {
    const that = this;
    let newPromise;
    // 处理参数默认值 保证参数后续能够继续执行
    onFulfilled =
        typeof onFulfilled === "function" ? onFulfilled : value =&gt; value;
    onRejected =
        typeof onRejected === "function" ? onRejected : reason =&gt; {
            throw reason;
        };

    // then里面的FULFILLED/REJECTED状态时 为什么要加setTimeout ?
    // 原因:
    // 其一 2.2.4规范 要确保 onFulfilled 和 onRejected 方法异步执行(且应该在 then 方法被调用的那一轮事件循环之后的新执行栈中执行) 所以要在resolve里加上setTimeout
    // 其二 2.2.6规范 对于一个promise，它的then方法可以调用多次.（当在其他程序中多次调用同一个promise的then时 由于之前状态已经为FULFILLED/REJECTED状态，则会走的下面逻辑),所以要确保为FULFILLED/REJECTED状态后 也要异步执行onFulfilled/onRejected

    // 其二 2.2.6规范 也是resolve函数里加setTimeout的原因
    // 总之都是 让then方法异步执行 也就是确保onFulfilled/onRejected异步执行

    // 如下面这种情景 多次调用p1.then
    // p1.then((value) =&gt; { // 此时p1.status 由pedding状态 =&gt; fulfilled状态
    //     console.log(value); // resolve
    //     // console.log(p1.status); // fulfilled
    //     p1.then(value =&gt; { // 再次p1.then 这时已经为fulfilled状态 走的是fulfilled状态判断里的逻辑 所以我们也要确保判断里面onFuilled异步执行
    //         console.log(value); // 'resolve'
    //     });
    //     console.log('当前执行栈中同步代码');
    // })
    // console.log('全局执行栈中同步代码');
    //

    if (that.status === FULFILLED) { // 成功态
        return newPromise = new Promise((resolve, reject) =&gt; {
            setTimeout(() =&gt; {
                try{
                    let x = onFulfilled(that.value);
                    resolvePromise(newPromise, x, resolve, reject); // 新的promise resolve 上一个onFulfilled的返回值
                } catch(e) {
                    reject(e); // 捕获前面onFulfilled中抛出的异常 then(onFulfilled, onRejected);
                }
            });
        })
    }

    if (that.status === REJECTED) { // 失败态
        return newPromise = new Promise((resolve, reject) =&gt; {
            setTimeout(() =&gt; {
                try {
                    let x = onRejected(that.reason);
                    resolvePromise(newPromise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
        });
    }

    if (that.status === PENDING) { // 等待态
        // 当异步调用resolve/rejected时 将onFulfilled/onRejected收集暂存到集合中
        return newPromise = new Promise((resolve, reject) =&gt; {
            that.onFulfilledCallbacks.push((value) =&gt; {
                try {
                    let x = onFulfilled(value);
                    resolvePromise(newPromise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
            that.onRejectedCallbacks.push((reason) =&gt; {
                try {
                    let x = onRejected(reason);
                    resolvePromise(newPromise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
        });
    }
};

/**
 * Promise.all Promise进行并行处理
 * 参数: promise对象组成的数组作为参数
 * 返回值: 返回一个Promise实例
 * 当这个数组里的所有promise对象全部变为resolve状态的时候，才会resolve。
 */
Promise.all = function(promises) {
    return new Promise((resolve, reject) =&gt; {
        let done = gen(promises.length, resolve);
        promises.forEach((promise, index) =&gt; {
            promise.then((value) =&gt; {
                done(index, value)
            }, reject)
        })
    })
}

function gen(length, resolve) {
    let count = 0;
    let values = [];
    return function(i, value) {
        values[i] = value;
        if (++count === length) {
            console.log(values);
            resolve(values);
        }
    }
}

/**
 * Promise.race
 * 参数: 接收 promise对象组成的数组作为参数
 * 返回值: 返回一个Promise实例
 * 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理(取决于哪一个更快)
 */
Promise.race = function(promises) {
    return new Promise((resolve, reject) =&gt; {
        promises.forEach((promise, index) =&gt; {
           promise.then(resolve, reject);
        });
    });
}

// 用于promise方法链时 捕获前面onFulfilled/onRejected抛出的异常
Promise.prototype.catch = function(onRejected) {
    return this.then(null, onRejected);
}

Promise.resolve = function (value) {
    return new Promise(resolve =&gt; {
        resolve(value);
    });
}

Promise.reject = function (reason) {
    return new Promise((resolve, reject) =&gt; {
        reject(reason);
    });
}

/**
 * 基于Promise实现Deferred的
 * Deferred和Promise的关系
 * - Deferred 拥有 Promise
 * - Deferred 具备对 Promise的状态进行操作的特权方法（resolve reject）
 *
 *参考jQuery.Deferred
 *url: http://api.jquery.com/category/deferred-object/
 */
Promise.deferred = function() { // 延迟对象
    let defer = {};
    defer.promise = new Promise((resolve, reject) =&gt; {
        defer.resolve = resolve;
        defer.reject = reject;
    });
    return defer;
}

/**
 * Promise/A+规范测试
 * npm i -g promises-aplus-tests
 * promises-aplus-tests Promise.js
 */

try {
  module.exports = Promise
} catch (e) {
}


</code></pre>
</html>

