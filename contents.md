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
- subscribeメソッドによりobsearvableが生成する値を受け取るcallbackを設定することができる
- subscribe時に実行される関数を保持している

Observer
- Observableをlistenするcallback関数(を保持するオブジェクト)
- callbackはnext, error, completeの3種類
- observableが値を生成した場合はnext, 例外が発生した場合はerror, ストリームが終了した場合はcompleteが呼ばれる

Operator
- ストリームをコレクションのように扱える(map, filter, concat, reduce)

and more...  https://rxjs-dev.firebaseapp.com/guide/overview

# Sample code with Observble

[Create observable object and subscribe it](https://codepen.io/anon/pen/yqprYw)

* subscribeがtriggerしobservableの関数を実行する
* subscribeした回数だけobservableの関数が実行される
  
[Observer can have three callback functions(next, error and complete)](https://codepen.io/anon/pen/VBzKWq)

* observableから新しい値が届くたびにnextが呼ばれる
* completeまたはerrorが呼ばれると、それ以降observableからnextが呼ばれることはない

[create another stream from stream](https://codepen.io/anon/pen/LBevNZ)
- operatorを組み合わせることでstreamから別のstreamを生成できる

# What's Observble

Observable represents stream and a stream is just a list expressed over time.

## Comparision with promise, function and iterator

||Single|Multiple|
|:-:|:-:|:-:|
|Pull|Function|Iterator|
|Push|Promise|Observable|

- 関数は１つの値を返す (同期)
- generatorはゼロ又は複数(無限個の可能性もある)の値を返す (同期)
- promiseは１つの値を返す (非同期)
- observableはゼロ又は複数(無限個の可能性もある)の値を返す (同期 or 非同期)

# Basic knowldge to use observable

## Marble diagram

- Marble diagramはストリームを表現するために使用される
- Operatorがどう動くかを理解する際やデバッグの際に有用

*図は
https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5 から

３つデータを生成し、完了したストリーム
![Emit data and complete](https://cdn-images-1.medium.com/max/1600/1*b-7_jU--CKfTkZ3hL66U6Q.png)

３つデータを生成し、エラーになったストリーム
![Emit data and error](https://cdn-images-1.medium.com/max/1600/1*DxXNdInXrcKT0Jg3WdGafQ.png)

ストリームからfilter operatorを通して別のストリームを生成するケース
![filter](https://cdn-images-1.medium.com/max/2000/1*t7F6N5eo7IQiq44VkjQMQQ.png)

https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5

## Operator

ストリームをコレクションのように扱える(map, filter, concat, reduce)便利関数

*図は
https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5 から

### map
![map](https://cdn-images-1.medium.com/max/1600/1*LNmVKOum63rRnln1fGM7dA.png)

### distinctUntilChanged

![distinctUntilChanged](https://cdn-images-1.medium.com/max/2000/1*4NeoXvtUntbo9uCcFpIZpg.png)

### combineLatest

![combineLatest](https://cdn-images-1.medium.com/max/2000/1*a41bu3YdfJSCcRub5oWplA.png)

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

エラーが発生した場合はobserverのerrorコールバックが呼ばれsubscriptionが自動的に解除される.

Observable側でエラー処理を定義する場合は以下のoperatorが利用できる.
- catch
- catchError
- retry
- retryWhen

### catch / catchError

エラーをcatchして代替のストリーム(observable)を返す場合に使用する.
catch時に可能な処理は以下.

- Errorをrethrowする(observerのerrorが呼ばれる)
- 代替のobservableを返す
    - エラー時の値を返して処理を継続させる
    - Rx.Observable.empty()を返してcompleteさせる
    - Rx.Observable.never()を返してhangさせる
  
https://codepen.io/anon/pen/EpErgy

* catchErrorはpipe operatorの時に使う??

### finally

error又はcomplete状態になるときに必ず呼ばれる.
**Observerのerror callbackの後に呼ばれる**
https://codepen.io/anon/pen/GBdPZo


### retry

引数で指定した回数分即時retryしてくれる.
https://codepen.io/anon/pen/mjLare

retryとcatchを組み合わせて、retryしても失敗した場合にエラー処理の実行をすることもできる.

### retryWhen

一定期間retry間隔を遅らせる場合に使える. 
回数を制限する場合はtakeを使うっぽい
https://codepen.io/anon/pen/oMdJYK?editors=0012
  

https://alligator.io/rxjs/simple-error-handling/

## Hot vs Cold Observables

Observableはストリームの生成のタイミングによりHot observableとCold observableに分類される.

[Cold observable](https://codepen.io/anon/pen/JBBGLx)
- subscribeのタイミングでストリームが生成され、subscribe毎に個別のストリームを持つ

[Hot observable](https://codepen.io/anon/pen/WKKrKO)
- subscribeの前から存在するストリーム(mouse move eventなど)から値をemitする.

[share operatorを利用](https://codepen.io/anon/pen/djjGBP)してCold observableを複数のsubscriberで共有する
- Cold observableをsubscribeしたタイミングでストリームが生成し、生成したストリームを他のsubscriberで共有可能

[shareReplayを使う](https://codepen.io/anon/pen/KBBVOo)
- share operatorだと最初のsubscribeのタイミングでemitされた値は他のsubscriberと共有できない(他のsubscriberがまだsubscribeしていないタイミングなので).
- shareReplayを使うことで直近でemitされた値をsubscribe時に流すことが可能
  
## How to Debug
- do (or tap) operator を使ってconsole.logに出していく
- Dependency graphやMarble diagramを書いて整理する

https://staltz.com/how-to-debug-rxjs-code.html

# Reference

[Reactive Extension公式](http://reactivex.io/)

[Angular公式](https://angular.io/)

[Codepen](https://codepen.io/)

[Understanding Marble Diagrams for Reactive Streams](https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5 )

[COLD VS HOT OBSERVABLES - thoughtram](https://blog.thoughtram.io/angular/2016/06/16/cold-vs-hot-observables.htm)

[Cold vs. Hot Observables](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/gettingstarted/creating.md#cold-vs-hot-observables)

[Rx Visualizer](https://rxviz.com/)