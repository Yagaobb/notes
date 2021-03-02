# setTimeout的第三个参数作用

作者：Coderfei
链接：https://juejin.cn/post/6844903831789109256
来源：掘金

##  题目：每隔1秒输出一个数字，从0-5
```
for(var i = 0; i<6; i++){
    setTimeout(function(){
        console.log(i);
    },1000);
}

因为setTimeout是一个异步操作，而等到执行setTimeout时，for循环已经执行完毕，这时的i已经等于6，所以输出6次的6。  
```
## 解决方法：
```
1. 利用闭包

for(var i=0;i<=5;i++){            
    (function(j){                
        setTimeout(function timer(){                    
            console.log(j)                
        },j*1000)            
    })(i)        
}

在上述代码中，我们首先使用了立即执行函数将 i 传入函数内部，这个时候值就被固定在了参数 j上面不会改变，当下次执行 timer 这个闭包的时候，就可以使用外部函数的变量 j，从而达到目的。
```

```
2. 使用 setTimeout 的第三个参数，这个参数会被当成 timer 函数的参数传入。
for(var i=0;i<=5;i++){           
    setTimeout((j) => {                
        console.log(j);            
    },i*1000,i)        
}



// setTimeout还允许更多的参数。它们将依次传入推迟执行的函数（回调函数）。
setTimeout((a,b,c) => {            
    console.log(a,b,c)        
}, 2000, "my", "name", "is starsion");
//my name is starsion
```

```
3. 使用 let 定义 i 了来解决问题了，这个也是最为推荐的方式

for(let i=0;i<=5;i++){                
    setTimeout(() => {                    
        console.log(i)                
    },i*1000)            
}
```

```
var i=0;
setTimeout(function(){
    console.log('第二次'+i)
},3000,setTimeout(function(){
    console.log('第一次'+i);
    i++;
},1000));

最后依次输出为 第一次0 第二次1 可以看到第三个参数还可以是先执行，然后再执行函数。 
```
