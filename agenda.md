# はじめに

## 目的
- Angularでは所々でRXJSが使われているが、RXJS内で使われる概念が独特なので、ある程度知らないとコードの開発・保守が辛い
- RXJSでハマる人をなるべく減らすため

## 対象
- AngularでRXJSをなんとなく使ってる人

## ゴール
- Angularを使う上で最低限必要(そう)なRXJSの知識が身についている

## 触れないこと
- RPとかFRPとかについてはあまりあまりふれない

## 方針

オンラインエディタを使っていじれるサンプルを使用する

https://codepen.io/anon/pen/VBzKWq

# RXJSの概要

- Reactive Extensionの簡単な紹介
    - Reactive Extension is 何?
    - どこからきたの
    - メリット
    - どんなフレームワークで使われてるの
- Angular内でRXJSがどう使われているのか
    - どこで使われるのか
- RXJSが提供する機能
    - Observable, utility function for observable
  
# Observble使用例
- ObservableからObservableを作っていくのがわかる例
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