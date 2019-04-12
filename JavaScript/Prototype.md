# JS的原型鏈

JS在設計時每一個變數都是Object，可以透過new的方式建立一個instance，該instance就會繼承Object，這就是透過原型鏈來實作「繼承」

#### 建立一個Person的function

```
function Person(n, a) {
    this.name = n;
    this.age = a;
}
Person.prototype.log = function() {
    console.log(this.name + this.age);
}
Person.prototype.from = "Taiwan";
```

![Person.prototype](https://github.com/ninolin/note/blob/master/JavaScript/images/Person.prototype.png)

#### new一個Person出來

每一個new出來的Person有自己的name和age，但是prototype是共享的，去改了prototype所有的用Person new出來的instance都會改變
```
var lou = new Person('lou', 25);
var becky = new Person('becky', 25);
Person.prototype.from = "Korea";
console.log(lou.name);   //lou
console.log(lou.from);   //Korea
console.log(becky.name); //becky
console.log(becky.from); //Korea
```
new出來的lou是靠__proto__跟Person的prototype來做關連的，這就是原型鏈
```
lou.__proto__ == Person.prototype
```
#### 利用Call來做一個繼承Person的function
利用call把Person變成staff的this繼承Person的如果跟Person有一樣的屬性
```
function staff(name, age) {
    Person.call(this, name, age);
    this.from = "USA";
}
var stf = new staff('becky', 25);
console.log(stf.name);   //becky
console.log(stf.from);   //USA
console.log(lou.from);   //Korea
```
