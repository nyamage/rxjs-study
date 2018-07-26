# 調査項目

- Reactive Programming
- RXJSとは
- stream
- subject
- observable
- operator
- Hot vs Cold Observables

# streamとは

> A stream is just a list expressed over time.

> In Rx (reactive extensions) nomenclature, you create observable streams and then process those streams using a set of functional utilities


# Promise vs Observable

> A promise is basically a stream that only emits a single value (or rejection). Observables can replace promises in your code and provide all of the standard functional utilities you may have grown accustomed to with libraries like Underscore or Lo-Dash.

# Reactive Programming

## How to Stop Micromanaging Everything
> In OO and imperative programming, we micromanage everything. We micromanage state, iteration counters, the timing of what happens when using things like event emitters and callbacks. What If I told you that all of that work is completely unnecessary — that you could literally delete entire categories of code from your programs?

> Reactive programming uses functional utilities like map, filter, and reduce to create and process data flows which propogate changes through the system: hence, reactive. When input x changes, output y updates automatically in response.

> In OO, you might set up some object (say, a form input), and turn that object into an event emitter, and set up an event listener that jumps through some hoops and maybe produces some output whenever the event is triggered.

> When using reactive programming, you instead specify data dependencies in a more declarative fashion, and most of the heavy lifting is offloaded to standard functional utilities so you don’t have to reinvent the wheel over and over again.

https://medium.com/javascript-scene/the-two-pillars-of-javascript-pt-2-functional-programming-a63aa53a41a4



OOではオブジェクト間の関係を記述していく (本文ではもっと手続きっぽいことをするように書かれているが)
RPではデータの依存性を記述していく

=> 同じことな気もする

# terminology

```
const post$ = Rx.Observable.of({id: 1});
const getPostInfo$ = Rx.Observable.timer(3000).mapTo({title: "Post title"});

const posts$ = post$.mergeMap(post => getPostInfo$).subscribe(res => console.log(res));
```

- Source ( or outer ) Observable
    - post$ is source Observable
- Inner Observable
    - getPostInfo$

## Hot vs Cold Observables

https://github.com/ichpuchtli/awesome-rxjs#hot-vs-cold-observables
https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339

## Subject

Subject can act as a bridge/proxy between the source Observable and many observers, making it possible for multiple observers to share the same Observable execution.

https://netbasal.com/understanding-subjects-in-rxjs-55102a190f3


# Rxjs in Angular

## observable

Event emitter

> Angular provides an EventEmitter class that is used when publishing values from a component through the @Output() decorator. EventEmitter extends Observable, adding an emit() method so it can send arbitrary values. When you call emit(), it passes the emitted value to the next() method of any subscribed observer.

Async Pipe

> The AsyncPipe subscribes to an observable or promise and returns the latest value it has emitted. When a new value is emitted, the pipe marks the component to be checked for changes.

https://angular.io/guide/observables-in-angular

## observable data service

> An observable data service is an Angular injectable service that can be used to provide data to multiple parts of the application. The service, that can be named a store can be injected in any place where the data is needed:

How to use an observable data service
> The data service exposes an observable, for example TodoStore exposes the todos observable

How to modify the data of a service
> The data in services is modified by calling action methods on them


Benefit of Observable data services
> Observable data services or stores are a simple and intuitive pattern that allows tapping into the power of functional reactive programming in Angular without introducing too many of new concepts.

https://blog.angular-university.io/how-to-build-angular2-apps-using-rxjs-observable-data-services-pitfalls-to-avoid/


# Angular Service Layers: Redux, RxJs and Ngrx Store - When to Use a Store And Why?

> Did you notice that there is a lot of information on store solutions, but limited information about when should we use them and why? 


## When to use Redux or stores in general?

https://blog.angular-university.io/angular-2-redux-ngrx-rxjs/

> You’ll know when you need Flux. If you aren’t sure if you need it, you don’t need it.


## When is it recommended to use Flux, or Redux?

data mode is hierarchical
> React components are arranged in a hierarchy. Most of the time, your data model also follows a hierarchy. In these situations Flux doesn’t buy you much

data model is not hierarchical
> your data model is not hierarchical. When your React components start to receive props that feel extraneous, or you have a small number of components starting to get very complex, then you might want to look into Flux


A store-like architecture is recommended if
> You have a piece of data that needs to be used in multiple places in your app, and passing it via props makes your components break the single-responsibility principle (i.e. makes their interface make less sense)

> There are multiple independent actors (generally, the server and the end-user) that may mutate that data


## Extraneous props, what else can it mean?

> The extraneous props issue seems to be a component inter-communication issue.

> There are many more examples. If we only had props or @Input() as a component communication mechanism we would run into trouble very quickly. Passing only inputs to components won't scale in complexity.

## Stores are a compound solution for multiple problems, not just one

stores are a multi-responsibility solution:

> they solve the problem of component interaction via the Observable pattern
they provide a client-side cache if needed, to avoid doing repeated Ajax requests
They provide a place to put temporary UI state, as we fill in a large form or want to store search criteria in a search form when navigating between router views
and they solve the problem of allowing modification of client side transient data by multiple actor


[TODO]You Might Not Need Redux
https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367


# Reference

[The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

- reactive programmingの基本的な考えが書かれている

[Ben Lesh](https://medium.com/@benlesh)

[RxJS — Six Operators That you Must Know](https://netbasal.com/rxjs-six-operators-that-you-must-know-5ed3b6e238a0)

- operatorの例はとてもわかりやすかった