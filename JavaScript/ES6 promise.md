# ES6关于promise
## 1.0 Proimise对象
Promise对象有3种状态：

- Pending--进行中
- Resolved--完成
- Rejected--已失败

一旦状态改变，就不会再变，任何时候都可以得到一个结果，失败或者成功。

## 1.1创建Promise实例

	方法1：

	let promise = new Promise(function(resolve, reject){
		
		// 异步操作成功
		if( 操作成功 )
		{
			resolve();
		}
		else {
			
			// 异步操作失败
			reject();
		}
		

	});

	// 方法2
	function promise () {
	    return new Promise ( function (resolve, reject) {
	        if ( success ) {
	            resolve(传递给事件成功的参数)
	        } else {
	            reject(传递给事件失败的参数)
	        }
	    } )
	}

Promise实例生成以后， 可以用then方法分别指定Resolved状态和Reject状态的回调函数。then()的第一个参数是运行resolve()函数时执行的函数，第二个参数是事件失败运行reject()时会运行的函数，这个参数是可选的。

	promise.then(function(value) {
		// value是 resolve()传递过来的参数
		// success
	}, function(error) {
		// error是 reject()传递过来的参数
		// failure
	});

ES6写法：

	promise.then(
	    (msg) => { console.log('this is success callback' + msg) },
	    (msg) => { console.log('this is fail callback' + msg) }
	)

注意：实例化的Promise对象会立即执行

    let promise = new Promise(function(resolve, reject) {
        console.log('Promise');
        resolve("this is event success");
    });
    
    promise.then(function(msg) {
        console.log(msg);
    });

    console.log('Hi!');

####输出：

Promise

Hi!

this is event success

Promise状态的改变是由用户自己设置的，如上面的程序中只运行了resovle()函数，表示成功状态由Pending-->Resolved 事件完成。一般这个函数是在一定条件才执行的，也就是用户操作成功时执行。当执行这个函数时，就会执行.then()函数第一个参数那个函数，也就是所谓的成功回调函数。
reject()函数也一样，都是由用户执行的。这个函数应该是在事件失败时才会执行，执行reject()函数时会执行then()函数的第二个参数那个函数，也就是事件失败时运行的回调函数。

由于Promise 一旦状态改变，就不会再变，所以任何时候都只能得到一个结果，失败或者成功。

所以下面的代码只会输出 ： reject msg: this is event fail

    let promise = new Promise(function(resolve, reject) {

        setTimeout( () => reject("this is event fail"), 1000);
        setTimeout( () => resolve("this is event success"), 2000);
        
    });
    
    promise.then(
        (msg) => { console.log("resolve msg: " + msg) },
        (msg) => { console.log("reject msg: " + msg) }
    );


## 1.2 Promise中的catch()方法

catch()的作用是捕获Promise的错误，与then()的rejected回调作用几乎一致。但是由于Promise的抛错具有冒泡性质，能够不断传递，这样就能够在下一个catch()中统一处理这些错误。同时catch()也能够捕获then()中抛出的错误，所以建议不要使用then()的rejected回调，而是统一使用catch()来处理错误。

	promise.then(
	    () => { console.log('this is success callback') }
	).catch(
	    (err) => { console.log(err) }
	)

如果在then()中处理了错误/失败的信息，catch()中就不会再处理错误信息了。


    let promise = new Promise(function(resolve, reject) {

        setTimeout( () => reject("this is event fail"), 1000);
        
    });
    
    promise.then(
        (msg) => { console.log("resolve msg: " + msg) },
        (msg) => { console.log("reject msg: " + msg) }
    ).catch(
        (err) => {
            console.log("error msg: " + err)
        }
    );

####只输出： reject msg: this is event fail


由于then() 和 catch() 都会返回一个**新的Promise对象**，因此可以链式调用。

	promise.then(
    	() => { console.log('this is success callback') }
	).catch(
	    (err) => { console.log(err) }
	).then(
	    ...
	).catch(
	    ...
	)

这些then()会顺序调用，但是如果内部有setTimeout()函数，就会先执行后面代码，之后再运行之前setTimeout()内部的代码。


	let promise = new Promise(function(resolve, reject) {
       
        setTimeout( () => resolve("event success"), 1001);
        
    });
    
    promise.then(
        ( msg ) => { 
            console.log("resolve1: " + msg);
            return "then 1 return msg"; 
        }

    ).then(
        ( msg ) => { 
            setTimeout(
                () => {
                    console.log("resolve2: " + msg);
                    return "then 2 return msg"; 
                },
                1000
            );
            //y+2;
        }
    ).then(
        ( msg ) => { console.log("resolve3: " + msg) }
    )
    .catch(
        (err) => {
            console.log("error msg: " + err)
        }
    );

输出：

resolve1: event success

resolve3: undefined

resolve2: then 1 return msg

这说明后面的then()函数会先执行，由于前面的then()还没有返回值，所以会输出 resolve3: undefined


## 1.3 Promise.all()

这是个Promise.all()实例

var p = Promise.all([p1, p2, p3]);

Promise.all()接收数组作为参数。p1 p2 p3都是Promise对象，如果不是就会先调用Promise.resolve方法，将参数转为Promise实例，再进一步处理。 

p的状态由p1、 p2、 p3决定， 分成两种情况。

（1） 只有p1、 p2、 p3的状态都变成fulfilled，也就是都运行了resolve()函数，p的状态才会变成fulfilled，此时p1、 p2、 p3的返回值组成一个数组，传递给p的回调函数。

（2） 只要p1、 p2、 p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

下面是都返回resolve()例子：

    let p1 = new Promise(function(resolve, reject) {
        
        setTimeout( () => resolve("p1 event success"), 1000);
    });

     let p2 = new Promise(function(resolve, reject) {
        
        setTimeout( () => resolve("p2 event success"), 2000);
    });

    let p3 = new Promise(function(resolve, reject) {
      
        setTimeout( () => resolve("p3 event success"), 3000);
    });

    let p = Promise.all([p1, p2, p3]);

    p.then(
        (msg) => { 
           
            console.log(msg);
            console.log("resolve1: " + msg);
        },
        (msg) => { 
            console.log("reject msg: " + msg); 
        }
    );

上面例子会在3S后才有数据输出：

["p1 event success", "p2 event success", "p3 event success"]

 resolve1: p1 event success,p2 event success,p3 event success


下面是有返回reject()例子：


    let p1 = new Promise(function(resolve, reject) {
        
        setTimeout( () => reject("p1 event fail"), 1000);
    });

     let p2 = new Promise(function(resolve, reject) {
        
        setTimeout( () => reject("p2 event fail"), 2000);
    });

    let p3 = new Promise(function(resolve, reject) {
      
        setTimeout( () => resolve("p3 event success"), 3000);
    });

    let p = Promise.all([p1, p2, p3]);

    p.then(
        (msg) => { 
            console.log(msg);
            console.log("resolve1: " + msg);
        },
        (msg) => { 
            console.log("reject msg: " + msg); 
        }
    );


这个例子会在1S后直接输出数据：

reject msg: p1 event fail

其他的数据都不会再输出。


## 1.4 Promise.race()

Promise.race方法同样是将多个Promise实例， 包装成一个新的Promise实例。

	var p = Promise.race([p1,p2,p3]);

只是与all()不同，race()方法只要p1、 p2、 p3之中有一个实例率先改变状态(不管失败或者成功，只要有一个状态发生改变就可以)，p的状态就跟着改变。 那个率先改变的Promise实例的返回值，就传递给p的回调函数。


## 1.5 Promise.resolve()

Promise.resolve()方法可以将一般的对象转成Promise对象。

	Promise.resolve('foo')
	// 等价于
	new Promise(resolve => resolve('foo'))































