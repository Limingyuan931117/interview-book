## Promise

>  是一种异步编程的解决方案， 为了解决异步的回调地狱



### Promise.resolve(value)

> 返回一个以value值解析后的Promise对象

1. 如果这个值是thenable（带有then方法），返回Promise对象，采用它的最终状态（resoleved/rejected/pending/settled）
2. 如果传入的value本身就是Promise对象， 则改对象作为Promise.resolve方法的返回值返回
3. 其他情况以该值为成功状态返回一个Promise对象

### Promise.reject

> 拒绝，返回的状态是reject

### Promise.prototype.then   |  Promise.prototype.catch

> 和平时方法一致

### Promise.race

> 多个Promise任务同时执行，返回最先执行结束的Promise任务的结果，不管这个是成功还是失败

### Promise.all

> 多个Promise同时进行
>
> 如果全部成功执行，则以数组的方式放回所有Promise任务的执行结果
>
> 如果有一个Promise为reject，则返回reject结果



14102 - 14105

