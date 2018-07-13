### RxJS名词解释及其使用

```
参考资源:http://blog.csdn.net/tianjun2012/article/details/51246422
        https://segmentfault.com/a/1190000005051034
        https://segmentfault.com/a/1190000005069851
        https://segmentfault.com/a/1190000005059624
```

RxJS(Reactive Extensions for JavaScript):Javascript的响应式扩展。响应式的思路是把随时间不断变化的数据、状态、事件等等转成可被观察的序列(Observable Sequence)，然后订阅序列中那些Observable对象的变化，一旦变化，就会执行事先安排好的各种转换和操作。
Rx = Observables + LINQ + Schedulers
适用场景：1.异步操作重，2.同时处理多个数据源。

1) Observable

字面意思是可被观察的,通常将其比拟为一个stream流.流是包含了有时序，正在进行事件的序列，可以发射(emmit)值(某种类型)、错误、完成信号.

一个Observable对象可以被观察者订阅,这样在Observable对象发生事件时,观察者就可以获得通知,从而作出相应的反应.这样就实现了事件产生与处理的分离.

通过使用Rx.Observable.create或者是创建操作符,创建一个Observable;Observable被Observer(观察者)订阅;在执行时向观察者发送next/error/complete通知;同时执行过程可以被终止.

2) Observer

Observer（观察者）是Observable（可观察对象）推送数据的消费者。在RxJS中，Observer是一个由回调函数组成的对象，键名分别为next、error 和 complete，以此接受Observable推送的不同类型的通知，下面的代码段是Observer的一个示例：

```
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};
```

在RxJS中，Observer是可选的。在next、error 和 complete处理逻辑部分缺失的情况下，Observable仍然能正常运行，为包含的特定通知类型的处理逻辑会被自动忽略。

下面例子中Observer并不包含complete类型通知的处理逻辑：

```
var observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
}
```

在订阅Observable时，你甚至可以把回调函数作为参数传入，而不是传入完整的Observer对象：

```
observable.subscribe(x => console.log('Observer got a next value: ' + x));
```

在RxJS内部，调用observable.subscribe时，它会创建一个只有next处理逻辑的Observer。当然你也可以将next、error 和 complete的回调函数分别传入：

```
observable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  err => console.error('Observer got an error: ' + err),
  () => console.log('Observer got a complete notification')
);
```

3) Subscription

Subscription是一个代表可以终止资源的对象，表示一个Observable的执行过程。Subscription有一个重要的方法：unsubscribe。这个方法不需要传入参数，调用后便会终止相应的资源。在RxJS以前的版本中，Subscription被称为"Disposable"。

```
var observable = Rx.Observable.interval(1000);
var subscription = observable.subscribe(x => console.log(x));
subscription.unsubscribe();
```

Subscription能够通过unsubscribe() 函数终止Observable的执行过程并释放相应资源。

4) Subject

在RxJS中，Subject是一类特殊的Observable，它可以向多个Observer多路推送数值。普通的Observable并不具备多路推送的能力（每一个Observer都有自己独立的执行环境），而Subject可以共享一个执行环境。

每一个Subject都是一个Observable（可观察对象）

对于一个Subject，你可以订阅（subscribe）它，Observer会和往常一样接收到数据。从Observer的视角看，它并不能区分自己的执行环境是普通Observable的单路推送还是基于Subject的多路推送。

Subject的内部实现中，并不会在被订阅（subscribe）后创建新的执行环境。它仅仅会把新的Observer注册在由它本身维护的Observer列表中，这和其他语言、库中的addListener机制类似。

每一个Subject也可以作为Observer（观察者）

Subject同样也是一个由next(v)，error(e)，和 complete()这些方法组成的对象。调用next(theValue)方法后，Subject会向所有已经在其上注册的Observer多路推送theValue。