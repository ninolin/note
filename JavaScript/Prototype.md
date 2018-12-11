# 介紹 Prototype

JS中每一種型態的變數都有Prototype這個物件，但只有function才會公開Prototype

#### 一個車子的function和把行為物件綁上去

```
function car(b, w) {
    this.brand = b;
    this.wheels = w;
}

var transportation = {
    goAhead : function() {
        console.log(this.brand + '在前進')
    },
    backWard: function() {
        console.log(this.brand + '在退後')
    }
}
```