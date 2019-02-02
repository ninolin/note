# JS的原型鏈

JS在設計時每一個變數都是Object，可以透過new的方式建立一個instance，該instance就會繼承Object的function，這就是透過原型鏈來實作「繼承」

#### 建立一個Person的function

```
function Person(n, a) {
    this.name = n;
    this.age = a;
}
Person.prototype.log = function() {
    console.log(this.name + this.age);
}
```
![Person.prototype](https://github.com/ninolin/note/blob/master/JavaScript/images/Person.prototype.png)