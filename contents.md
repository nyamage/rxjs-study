# はじめに

## 目的
- Angularでは所々でRXJSが使われているが、RXJS内で使われる概念が独特なので、ある程度知らないとコードの開発・保守が辛い
- RXJSでハマる人をなるべく減らすため

## 対象
- AngularでRXJSをなんとなく使ってる人

## ゴール
- Angularを使う上で最低限必要(そう)なRXJSの知識が身についている

## 話さないこと
- RPとかFRPとかについてはあまりあまりふれない

## 方針

オンラインエディタを使っていじれるサンプルを使用する

https://codepen.io/anon/pen/VBzKWq

# Overview

## What's Reactive Extension?

- **ストリーム**(非同期イベント)処理のためのライブラリ

## Whrere does RXEX come from?

- C#のライブラリとして開発され、現在は[様々な言語](http://reactivex.io/languages.html#languages)で利用できる

## Usage in Angular2
- EventEmitter
- HTTP module 
- The Router and Forms modules
  
https://angular.io/guide/observables-in-angular

## What does rxjs provide?

Observable
- ストリームを表現するオブジェクト
- RXJSにおいてコアとなる型
- subscribe時に実行される関数を保持しているだけ
  
Observer
- Observableをlistenするcallback関数(を保持するオブジェクト)

Operator
- ストリームをコレクションのように扱える(map, filter, concat, reduce)

and more...  https://rxjs-dev.firebaseapp.com/guide/overview

# Sample code with Observble

[1] Create observable object and subscribe it
https://codepen.io/anon/pen/yqprYw?editors=0012

* subscribeがtriggerしobservableの関数を実行する
* subscribeした回数だけobservableの関数が実行される
  
[2] Observer can have three callback functions(next, error and complete).
https://codepen.io/anon/pen/VBzKWq?editors=0012

* observableから新しい値が届くたびにnextが呼ばれる
* completeまたはerrorが呼ばれると、それ以降observableからnextが呼ばれることはない

[3] create new stream from stream
https://codepen.io/anon/pen/LBevNZ?editors=0012

# What's Observble

Observable represents stream and a stream is just a list expressed over time.

## Comparision with promise and function, iterator

||Single|Multiple|
|:-:|:-:|:-:|
|Pull|Function|Iterator|
|Push|Promise|Observable|

- 関数は１つの値を返す (同期)
- generatorはゼロ又は複数(無限個の可能性もある)の値を返す (同期)
- promiseの値は１つの値を返す (非同期)
- observableはゼロ又は複数(無限個の可能性もある)の値を返す (同期 or 非同期)

# Basic knowldge to use observable

## Marble diagram

- Marble diagramはストリームを表現するために使用される
- Operatorがどう動くかを理解する際やデバッグの際に有用

### Marble diagram sample

３つデータを生成し、完了したストリーム
![Emit data and complete](https://cdn-images-1.medium.com/max/1600/1*b-7_jU--CKfTkZ3hL66U6Q.png)

３つデータを生成し、エラーになったストリーム
![Emit data and error](https://cdn-images-1.medium.com/max/1600/1*DxXNdInXrcKT0Jg3WdGafQ.png)

ストリームからfilter operatorを通して別のストリームを生成するケース
![filter](https://cdn-images-1.medium.com/max/2000/1*t7F6N5eo7IQiq44VkjQMQQ.png)

https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5

## Operator

ストリームをコレクションのように扱える(map, filter, concat, reduce)便利関数

### map
![map](https://cdn-images-1.medium.com/max/1600/1*LNmVKOum63rRnln1fGM7dA.png)

### distinctUntilChanged

![distinctUntilChanged](https://cdn-images-1.medium.com/max/2000/1*4NeoXvtUntbo9uCcFpIZpg.png)

### combineLatest

![combineLatest](https://cdn-images-1.medium.com/max/2000/1*a41bu3YdfJSCcRub5oWplA.png)

図は
https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5 から

### mergeMap(flatMap)

observableの値をtriggerにストリームを作る(メタストリーム)例
![map](https://camo.githubusercontent.com/2a8a9cc75acd13443f588fd7f386bd7a6dcb271a/687474703a2f2f692e696d6775722e636f6d2f48486e6d6c61632e706e67)

mergeMapを利用するとメタストリームの値をOutputのストリームに出力することができる
![mergeMap](https://cdn-images-1.medium.com/max/2000/1*HnjrvlaOGvVRmSs1uSUiqA.png)

### concatMap

mergeMapと似てるが違う点は前のメタストリームの完了を待つかどうか
concatMapはメタストリームが完了してから次のメタストリームの値を出力する

![concatMap](https://cdn-images-1.medium.com/max/2000/1*0O1r-YUeJ3mncOnrZayV6Q.png)

## Error Handling

RXJSではエラーが発生した場合はobserverのerrorコールバックが呼ばれsubscriptionが自動的に解除される.

observable側でエラー処理を定義する場合は以下のoperatorが利用できる.
- catch
- catchError
- retry
- retryWhen

### catch

エラーをcatchして代替のストリーム(observable)を返す場合に使用する.
catch時に可能な処理は以下
- rethrow (errorになる)
- 代替のobservableを返す
    - エラー時の値を返す
    - Rx.Observable.empty()を返してcompleteさせる
    - Rx.Observable.never()を返してhangさせる
- 
https://codepen.io/anon/pen/EpErgy?editors=0012

### catchError

### retry

### retryWhen



- catch, catchError, retry, retryWhen
  

https://alligator.io/rxjs/simple-error-handling/

## Hot vs Cold Observables
- Hot observableの例
- Cold observableの例
- ハマるケース
  
## How to Debug
- Rxjsを使ってる場合のDebug手段として何があるか?

# Reference

[Reactive Extension公式](http://reactivex.io/)

[Angular公式](https://angular.io/)

[Codepen](https://codepen.io/)

[Understanding Marble Diagrams for Reactive Streams](https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5 )