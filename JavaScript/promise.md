# ES6 Promise 使用介紹

## 概述

Promise 是 ES6 設計用來處理 JavaScript 異步(async) function 的語法結構，概念是把要「異步執行的 function」變成 Promise 物件，就可以用 Promise 的語法來控制每一個 Promise 物件執行時機。

## Promise 物件

在 Promise 物件中要注意的是三種狀態「pending」「resolved」「rejected」的改變。
*   pending: 程式進入 Promise 物件時的狀態 (若沒有改變狀態的語法或錯誤發生時，就算程式跑完了狀態還是「pending」)
*   resolve: 使用語法「resolve()」讓物件結束，同時狀態變「resolve」
*   reject: 使用語法「reject()」或是程式有錯誤讓物件結束，同時狀態變「reject」

Promise 物件語法結構如下。

```
const promise = new Promise(function(resolve, reject) {
    console.log("Hello promise"); // 要異步執行的程式碼
    resolve(); //執行成功
    reject(); //執行失敗
});
```
## Promise 物件串連

可以透過 then 語法把多個 Promise 串連起來，當第一個 Promise 物件狀態變成 resolve 時，便會執行下一個 Promise 物件，同時可以把值傳給下一個 Promise。 

以下範例串連三個 promise 物件，同時把第一個物件的"HALOHA"傳到最後的物件並印出。

```
const promise = new Promise(function(resolve, reject) {
    console.log("Hello promise"); // 要異步執行的程式碼
    resolve("HALOHA"); //執行成功
})
.then((v) => {
    console.log("Hello promise"); // 要異步執行的程式碼
    return Promise.resolve(v);
})
.then((v) => {
    console.log(v);
});
```
