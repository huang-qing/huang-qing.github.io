---
layout:     post
title:      RxJS - Observable
subtitle:   
date:       2017-07-09
author:     huangqing
header-img: img/post-bg-javascript.jpg
catalog: true
categories: [javascript]
tags:
    - RxJS
---

# Observable

[reactivex:operators](http://cn.rx.js.org/manual/overview.html#h16)

[rxjs-china:快速查找操作器](http://rxjs-china.org/_book/)

[buctwbzs.gitbooks.io operators](https://buctwbzs.gitbooks.io/rxjs/content/operators.html)

[learnrxjs operators](https://www.learnrxjs.io/operators/)

## Operators [cn rx.js.org observable](http://cn.rx.js.org/class/es6/Observable.js~Observable.html)

**Combination Operators** 

+ combineAll
+ combineLatest :star:
+ concat :star:
+ concatAll
+ forkJoin
+ merge :star:
+ mergeAll
+ pairwise
+ race
+ startWith :star:
+ withLatestFrom :star:
+ zip

**Conditional Operators**

+ defaultIfEmpty 如果源Observable本来就是空的,那么这个操作符会发出一个默认值
+ every 每项都满足指定的条件

**Creation Operators**

+ create 创建一个新的 Observable 
+ empty  创建一个简单的只发出完成状态通知的 Observable 
+ from :star: 从一个数组、类数组对象、Promise、迭代器对象或者类 Observable 对象创建一个 Observable
+ fromEvent 建一个来自于 DOM 事件，或者 Node 的 EventEmitter 事件或者其他事件的 Observable
+ fromEventPattern 从一个基于 addHandler/removeHandler 方法的API创建 Observable
+ fromPromise :star:  将 Promise 转化为 Observable,返回一个仅仅发出 Promise resolve 过的值然后完成的 Observable
+ interval 定期发出自增的数字
+ of :star: 创建一个 Observable，它会依次发出由你提供的参数，最后发出完成通知
+ range 创建一个 Observable ，它发出指定范围内的数字序列
+ throw 仅仅发出 error 通知
+ timer 创建一个 Observable，该 Observable 在初始延时（initialDelay）之后开始发送并且在每个时间周期（ period）后发出自增的数字.就像是interval, 但是你可以指定什么时候开始发送。
+ never 创建一个不向观察者发出任何项的 Observable 
+ defer 创建一个 Observable，当被订阅的时候，调用 Observable 工厂为每个观察者创建新的 Observable。

**Error Handling Operators**

+ catch :star:
+ retry
+ retryWhen

**Multicasting Operators**

+ publish
+ multicast
+ share :star:

**Filtering Operators**

+ debounce 防抖动
+ debounceTime :star:
+ throttle 节流阀
+ throttleTime
+ distinct 去除重复
+ distinctUntilChanged :star: 泛型对比去除重复
+ distinctUntilKeyChanged 泛型对比去除重复，通过提供的 key 访问到的属性来检查两个项是否不同
+ filter :star: 条件过滤
+ first 第一个满足条件的值
+ last  最后一个满足条件的值
+ skip  跳过N个值
+ skipLast 跳过最后的N个值
+ skipWhile 跳过所有满足指定条件的数据项，但是一旦出现了不满足条件的项，则发出在此之后的所有项。
+ skipUntil 跳过源发出的值直到第二个 Observable 开始发送
+ single  匹配指定`predicate` 函数的单个项。 如果源发出多于1个数据项或者没有发出数据项, 分别以 `IllegalArgumentException` 和 `NoSuchElementException` 进行通知
+ take :star: 发出源最初发出的的N个值
+ takeLast 只发出源 Observable 最后发出的的N个值 
+ takeUntil :star: 发出源 Observable 的值，然后直到第二个 Observable (即 notifier )发出项，它便完成
+ takeWhile 当第一个不满足条件的值出现时，它便完成
+ sample 取样
+ ignoreElements 忽略源发送的所有项，只传递 `complete` 或 `error` 的调用。
+ find 找到第一个通过测试的值并将其发出
+ findIndex 只发出源 Observable 所发出的值中第一个满足条件的值的索引。它很像 find , 但发出的是找到的值的索引， 而不是值本身。

**Transformation Operators**

+ buffer
+ bufferCount
+ bufferTime :star:
+ bufferToggle
+ bufferWhen
+ concatMap :star:
+ concatMapTo
+ expand
+ groupBy
+ map :star: 把每个源值传递给转化函数以获得相应的输出值
+ mapTo
+ mergeMap :star:
+ partition
+ pluck
+ scan :star:
+ switchMap :star:
+ window
+ windowCount
+ windowTime
+ windowToggle
+ windowWhen

**Utility Operators**

+ do :star:
+ delay
+ delayWhen
+ dematerialize
+ let
+ materialize
+ observeOn
+ toPromise


RxJS提供了各种API来创建数据流：

+ 单值：of, empty, never
+ 多值：from
+ 定时：interval, timer
+ 从事件创建：fromEvent
+ 从Promise创建：fromPromise
+ 自定义创建：create

创建出来的数据流是一种可观察的序列，可以被订阅，也可以被用来做一些转换操作，比如：

+ 改变数据形态：map, mapTo, pluck
+ 过滤一些值：filter, skip, first, last, take
+ 时间轴上的操作：delay, timeout, throttle, debounce, audit, bufferTime
+ 累加：reduce, scan
+ 异常处理：throw, catch, retry, finally
+ 条件执行：takeUntil, delayWhen, retryWhen, subscribeOn, ObserveOn
+ 转接：switch

也可以对若干个数据流进行组合：

+ concat，保持原来的序列顺序连接两个数据流
+ merge，合并序列
+ race，预设条件为其中一个数据流完成
+ forkJoin，预设条件为所有数据流都完成
+ zip，取各来源数据流最后一个值合并为对象
+ combineLatest，取各来源数据流最后一个值合并为数组

### STATIC OPERATORS

`bindCallback`:将一个回调函数API转化为一个能返回一个Observable的函数

```javascript
public static bindCallback(func: function, selector: function, scheduler: Scheduler): function(...params: *): Observable
```

```javascript
// Convert jQuery's getJSON to an Observable API
// Suppose we have jQuery.getJSON('/my/url', callback)
var getJSONAsObservable = Rx.Observable.bindCallback(jQuery.getJSON);
var result = getJSONAsObservable('/my/url');
result.subscribe(x => console.log(x), e => console.error(e));
```

`bindNodeCallback`:将一个NodeJS风格的回调函数API转化为一个能返回可观察对象的函数.基本等同于bindCallback,不同的是作为输入函数的参数的回调函数要有error参数：callback(erro,result).

```javascript
public static bindNodeCallback(func: function, selector: function, scheduler: Scheduler): function(...params: *): Observable
```

```javascript
import * as fs from 'fs';
var readFileAsObservable = Rx.Observable.bindNodeCallback(fs.readFile);
var result = readFileAsObservable('./roadNames.txt', 'utf8');
result.subscribe(x => console.log(x), e => console.error(e));
```

`combineLatest`:组合多个Observable产生一个新的Observable，其发射的值根据其每个输入Observable的最新值计算.

![combineLatest](/images/rxjs/combineLatest.png)

```javascript
public static combineLatest(observable1: Observable, observable2: Observable, project: function, scheduler: Scheduler): Observable
```

```javascript
// Dynamically calculate the Body-Mass Index from an Observable of weight and one for height
var weight = Rx.Observable.of(70, 72, 76, 79, 75);
var height = Rx.Observable.of(1.76, 1.77, 1.78);
var bmi = Rx.Observable.combineLatest(weight, height, (w, h) => w / (h * h));
bmi.subscribe(x => console.log('BMI is ' + x));
```

`concat`:产生一个Observable，它取作为参数的Observable发射的每个值，并顺序(基于作为输入的可观察对象的顺序)发出所有值

```javascript
public static concat(input1: Observable, input2: Observable, scheduler: Scheduler): Observable
```

```javascript
// Concatenate a timer counting from 0 to 3 with a synchronous sequence from 1 to 10
var timer = Rx.Observable.interval(1000).take(4);
var sequence = Rx.Observable.range(1, 10);
var result = Rx.Observable.concat(timer, sequence);
result.subscribe(x => console.log(x));
```

`create`:创建一个新的Observable，当被订阅时，它将执行指定的函数

create将subscribe函数转换为实际的Observable。 这相当于调用Observable构造函数。 编写subscribe函数，使其作为一个Observable：它应该调用订阅者的next，error和complate方法,遵循Observable约束；良好的Observable必须调用Subscriber的complate方法一次或其error方法一次，然后再不会调用之后的next。

大多数时候，您不需要使用create，因为现有的创建操作符（以及实例组合运算符）允许您为大多数用例创建一个Observable。 但是，create是低级的，并且能够创建任何Observable。

```javascript
public static create(subscribe: function(subscriber: Subscriber): TeardownLogic): Observable
```

```javascript
var result = Rx.Observable.create(function (subscriber) {
  subscriber.next(Math.random());
  subscriber.next(Math.random());
  subscriber.next(Math.random());
  subscriber.complete();
});
result.subscribe(x => console.log(x));
```

`defer`:参数为一个Observable工厂函数，当被订阅时工厂函数被调用产生一个可观察对象。 以惰性的方式产生一个Observable,也就是说，当被订阅的时候才会产生

```javascript
public static defer(observableFactory: function(): Observable | Promise): Observable
```

```javascript
var clicksOrInterval = Rx.Observable.defer(function () {
  if (Math.random() > 0.5) {
    return Rx.Observable.fromEvent(document, 'click');
  } else {
    return Rx.Observable.interval(1000);
  }
});
clicksOrInterval.subscribe(x => console.log(x));
```

`empty`:创建一个不发射任何值的Observable,它只会发射一个complate通知

```javascript
public static empty(scheduler: Scheduler): Observable
```

```javascript
//Emit the number 7, then complete.
var result = Rx.Observable.empty().startWith(7);
result.subscribe(x => console.log(x));

//Map and flatten only odd numbers to the sequence 'a', 'b', 'c
var interval = Rx.Observable.interval(1000);
var result = interval.mergeMap(x =>
  x % 2 === 1 ? Rx.Observable.of('a', 'b', 'c') : Rx.Observable.empty()
);
result.subscribe(x => console.log(x));
```

`forkJoin`: 并行运行所有可观察序列并收集其最后的元素

```javascript
Rx.Observable.forkJoin(...args [resultSelector])
```

```javascript
/* Using observables */
var source = Rx.Observable.forkJoin(
    Rx.Observable.of(42),
    Rx.Observable.range(0, 10),
    Rx.Observable.from([1,2,3])
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => Next: [42, 9, 3]
// => Completed
```

`from`:将一个数组、类数组(字符串也可以)，Promise、可迭代对象，类可观察对象、转化为一个Observable

```javascript
public static from(ish: ObservableInput<T>, scheduler: Scheduler): Observable<T>
```

```javascript
// 数组=>Observable
var array = [10, 20, 30];
var result = Rx.Observable.from(array);
result.subscribe(x => console.log(x));
```

`fromEvent`:将一个元素上的事件转化为一个Observable

```javascript
Rx.Observable.fromEvent(element, eventName, [selector])
```

```javascript
var clicks = Rx.Observable.fromEvent(document, 'click');
clicks.subscribe(x => console.log(x));
```

```javascript
var input = $('#input');

var source = Rx.Observable.fromEvent(input, 'click');

var subscription = source.subscribe(
    function (x) {
        console.log('Next: Clicked!');
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

input.trigger('click');

// => Next: Clicked!
```

`fromEventPattern`:通过使用addHandler和removeHandler函数添加和删除处理程序。 当输出Observable被订阅时，addHandler被调用，并且当订阅被取消订阅时调用removeHandler。

```javascript
Rx.Observable.fromEventPattern(addHandler, [removeHandler], [selector])
```

```javascript
function addClickHandler(handler) {
  document.addEventListener('click', handler);
}

function removeClickHandler(handler) {
  document.removeEventListener('click', handler);
}

var clicks = Rx.Observable.fromEventPattern(
  addClickHandler,
  removeClickHandler
);
clicks.subscribe(x => console.log(x));
```

`fromPromise`:转化一个Promise为一个Obseervable

```javascript
public static fromPromise(promise: Promise<T>, scheduler: Scheduler): Observable<T>
```

```javascript
//convert the Promise returned by Fetch to an Observable

var result = Rx.Observable.fromPromise(fetch('http://myserver.com/'));
result.subscribe(x => console.log(x), e => console.error(e));
```

`interval`:返回一个以周期性的、递增的方式发射值的Observable

```javascript
public static interval(period: number, scheduler: Scheduler): Observable
```

```javascript
var numbers = Rx.Observable.interval(1000);
numbers.subscribe(x => console.log(x));
```

`of`:创建一个Observable，发射指定参数的值，一个接一个，最后发出complate

```javascript
public static of (values:...T,scheduler:Scheduler):Observable
```

```javascript
// Emit 1, 2, 3, then 'a', 'b', 'c', then start ticking every second.
var numbers = Rx.Observable.of(1, 2, 3);
var letters = Rx.Observable.of('a', 'b', 'c');
var interval = Rx.Observable.interval(1000);
var result = numbers.concat(letters).concat(interval);
result.subscribe(x => console.log(x));
```

`merge`:创建一个发射所有被合并的observable所发射的值

![merge](/images/rxjs/merge.png)

```javascript
public static merage(observable:...Observable,conurrent:number,scheduler:Schedule):Observable
```

```javascript
//Merge together 3 Observables, but only 2 run concurrently
//先运行timer1,timer2,执行完毕之后再执行timer3

var timer1 = Rx.Observable.interval(1000).take(10);
var timer2 = Rx.Observable.interval(2000).take(6);
var timer3 = Rx.Observable.interval(500).take(10);
var concurrent = 2; // the argument
var merged = Rx.Observable.merge(timer1, timer2, timer3, concurrent);
merged.subscribe(x => console.log(x));
```

`never`: 创建一个不发射任何值的Observable

```javascript
public static never():Observable
```

```javascript
function info() {
  console.log('Will not be called');
}
var result = Rx.Observable.never().startWith(7);
result.subscribe(x => console.log(x), info, info);
```


`range`: 创建发射一个数字序列的observable

```javascript
public static range(start:number,count:number,scheduler:Scheduler):Observable
```

```javascript
var numbers = Rx.Observable.range(1, 10);
numbers.subscribe(x => console.log(x));
```

`toAsync`:将函数转换为异步函数。 生成的异步函数的每次调用都会导致调用指定调度程序上的原始同步函数

```javascript
Rx.Observable.toAsync(function:Function,[scheduler],[context]):Function
```

```javascript
var func = Rx.Observable.toAsync(function (x, y) {
    return x + y;
});

// Execute function with 3 and 4
var source = func(3, 4);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 7
// => Completed
```

`throw`:创建一个只发出error通知的Observable。

```javascript
public static throw(error:any,scheduler:Scheduler):Observable
```

```javascript
var result = Rx.Observable.throw(new Error('oops!')).startWith(7);
result.subscribe(x => console.log(x), e => console.error(e));
```

`timer`:Creates an Observable that starts emitting after an initialDelay and emits ever increasing numbers after each period of time thereafter.

```javascript
public static timer(initialDelay: number | Date, period: number, scheduler: Scheduler): Observable
```

```javascript
//Emits ascending numbers, one every second (1000ms), starting after 3 seconds
var numbers = Rx.Observable.timer(3000, 1000);
numbers.subscribe(x => console.log(x));
```

`webSocket`:Wrapper around the w3c-compatible WebSocket object provided by the browser.

```javascript
public static webSocket(urlConfigOrSource: string | WebSocketSubjectConfig): WebSocketSubject
```

```javascript
let subject = Observable.webSocket('ws://localhost:8081');
subject.subscribe(
   (msg) => console.log('message received: ' + msg),
   (err) => console.log(err),
   () => console.log('complete')
 );
subject.next(JSON.stringify({ op: 'hello' }));
```

``:Combines multiple Observables to create an Observable whose values are calculated from the values, in order, of each of its input Observables.

If the latest parameter is a function, this function is used to compute the created value from the input values. Otherwise, an array of the input values is returned.

```javascript
public static zip(observables: *): Observable<R>
```

```javascript
//Combine age and name from different sources

let age$ = Rx.Observable.of(27, 25, 29);
let name$ = Rx.Observable.of('Foo', 'Bar', 'Beer');
let isDev$ = Rx.Observable.of(true, true, false);

Rx.Observable.zip(age$,
         name$,
         isDev$,
         (age, name, isDev) => ({ age, name, isDev })).subscribe(x => console.log(x));

// outputs
// { age: 27, name: 'Foo', isDev: true }
// { age: 25, name: 'Bar', isDev: true }
// { age: 29, name: 'Beer', isDev: false }
```

### INSTANCE OPERATORS

#### filter

`audit`:在某个持续时间段内忽略原始observable发射的值 ，该方法的参数为一个函数，该函数需返回一个决定持续时长 的observable或者promise。之后从原始observable发射最近的值，不断重复这个过程。audit很像auditTime，但是其持续时长是由第二个observable所决定。

audit：当另一个Observable发射值前，源Observable的值会被忽略，当另一个Observable发射值时，才从源Observable发射一个最新值，然后重复上述过程。

auditTime：在指定等待时间内，源Observable的值会被忽略，等待结束后，发射一个源Observable的最新值，然后重复上述过程。

audit 和 throttle 很像, 但是发出沉默时间窗口的最后一个值, 而不是第一个。只要 audit 的内部时间器被禁用， 它就会在输出 Observable 上发出源 Observable 的最新值，并且当时间器启用时忽略源值。初始时，时间器是禁用的。 只要第一个源值到达，时间器是用源值调用 durationselector 方法启用，返回 "duration" Observable。 当 duration Observable 发出数据或者完成时，时间器禁用，然后输出 Observable 发出最新的源值，并且不断的重复这个过程。

**他们与throttle的区别是，第一个值的发射，是先等待再发射，而throttle是先发射第一个值，然后再等待。**

```javascript
puiblic audit(durationSelector:function(value:T):Observable):Observable<T>
```

![audit](/images/rxjs/audit.png)

```javascript
// 每秒只会有一次单击会被发射，发射的时间点为每隔1秒
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.audit(ev => Rx.Observable.interval(1000));
result.subscribe(x => console.log(x));
```

`auditTime`:duration 毫秒内忽略源值，然后发出源 Observable 的最新值， 并且重复此过程。当它看见一个源值，它会在接下来的 duration 毫秒内忽略这个值以及接下来的源值，过后发出最新的源值。

```javascript
public auditTime(duration: number, scheduler: Scheduler): Observable<T>
```

```javascript
//以每秒最多点击一次的频率发出点击事件
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.auditTime(1000);
result.subscribe(x => console.log(x));
```

`throttle`(节流阀):从源 Observable 中发出一个值，然后在由另一个 Observable 决定的期间内忽略 随后发出的源值，然后重复此过程。它很像 throttleTime，但是沉默持续时间是由 第二个 Observable 决定的。

![throttle](/images/rxjs/throttle.png)

```javascript
public throttle(durationSelector: function(value: T): SubscribableOrPromise, config: Object): Observable<T>
```

```javascript
//以每秒最多点击一次的频率发出点击事件
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.throttle(ev => Rx.Observable.interval(1000));
result.subscribe(x => console.log(x));
```

`throttleTime`:从源 Observable 中发出一个值，然后在 duration 毫秒内忽略随后发出的源值， 然后重复此过程。让一个值通过，然后在接下来的 duration 毫秒内忽略源值。

```javascript
public throttleTime(duration: number, scheduler: Scheduler): Observable<T>
```

```javascript
//以每秒最多点击一次的频率发出点击事件
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.throttleTime(1000);
result.subscribe(x => console.log(x));
```

`debounce`:只有在另一个 Observable 决定的一段特定时间经过后并且没有发出另一个源值之后，才从源 Observable 中发出一个值。

```javascript
public debounce(durationSelector: function(value: T): SubscribableOrPromise): Observable
```

```javascript
//在一顿狂点后只发出最新的点击
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = clicks.debounce(() => Rx.Observable.interval(1000));
result.subscribe(x => console.log(x));
```

`sample`:发出源 Observable 最新发出的值当另一个 notifier Observable发送时。就像是 sampleTime， 但是无论何时notifier Observable 进行了发送都会去取样。

```javascript
public sample(notifier: Observable<any>): Observable<T>
```

![sample](/images/rxjs/sample.png)

无论何时 notifier Observable 发出一个值或者完成， sample 会去源 Observable 中发送上次 取样后源 Observable 发出的最新值， 除非源在上一次取样后没有发出值。 notifier会被订阅只要输出 Observable 被订阅。

```javascript
//每次点击， 取样最新的 "seconds" 时间器
var seconds = Rx.Observable.interval(1000);
var clicks = Rx.Observable.fromEvent(document, 'click');
var result = seconds.sample(clicks);
result.subscribe(x => console.log(x));
```