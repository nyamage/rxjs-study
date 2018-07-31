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

- **非同期イベント(ストリーム)**処理のためのライブラリ

## Whrere does RXEX come from?

- C#上のライブラリとして開発され、現在は[様々な言語](http://reactivex.io/languages.html#languages)で利用できる

## Usage in Angular2
- EventEmitter
- HTTP module 
- The Router and Forms modules
https://angular.io/guide/observables-in-angular

## What does rxjs provide?

Observable
- 非同期なデータのコレクションを表現するオブジェクト
- RXJSで使われるコアの型
  
Observer
- Observableをlistenするcallback関数(を保持するオブジェクト)

Operator
- 非同期イベントをコレクションのように扱える(map, filter, concat, reduce)

and more...  https://rxjs-dev.firebaseapp.com/guide/overview

# Observble使用例

例1) Observableはsubscribeしないと実行されない
https://codepen.io/anon/pen/yqprYw?editors=0012

例2) Observerはnext, error, completeのコールバックを持つオブジェクト
https://codepen.io/anon/pen/VBzKWq?editors=0012

例3) ObservableからObservableを作る
https://codepen.io/anon/pen/LBevNZ?editors=0012

- httpの例

# Observbleのもう少し深い説明
- ストリーム
- 非同期・多値, function, iterator, promiseとの比較
- marble diagram

# Observableを使う上で必要となる知識

## Operator
- 代表的なOperatorの例

## Error Handling
- catch, catchError, retry, retryWhen
  
## Hot vs Cold Observables
- Hot observableの例
- Cold observableの例
- ハマるケース
  
## How to Debug
- Rxjsを使ってる場合のDebug手段として何があるか?

# Reference