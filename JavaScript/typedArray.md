# TypedArray 使用介紹

TypedArray是JS在處理二進位檔(Binary Data)時會使用到資料型態，主要分為Buffer和View

* Buffer: 一段固定大小的二進位資料，需用View操作(ArrayBuffer)
* View: 用來操作Buffer(TypedArray、DataView)

## ArrayBuffer

一段固定大小的二進位資料，無法直接操作，可以把資料的記憶體位置reference給View進行操作

## TypedArray

TypedArray是用來操作Buffer的物件，目前有9種型別

型別                 | Bytes          
--------------------|:-------
Int8Array           | 1
Uint8Array          | 1 
Uint8ClampedArray   | 1 
Int16Array          | 2       
Uint16Array         | 2       
Int32Array          | 4       
Uint32Array         | 4
Float32Array        | 4
Float64Array        | 8

TypedArray是reference要操作的ArrayBuffer，所以一個ArrayBuffer可以被多個TypedArray操作，TypedArray可以使用map、filter、reduce等方法來操作ArrayBuffer的資料

```
var buffer = new ArrayBuffer(16);       //產生一個長度為16Bytes的二進位資料(值預設為0)
var int16View = new Int16Array(buffer); //用一個chunk大小為16bits(2Bytes)的TypedArray來操作
console.log(int16View.length)           //值為8，因為16/2=8
console.log(int16View);                 //Int16Array(4) [0, 0, 0, 0, 0, 0, 0, 0]
for (var i = 0; i < int16View.length; i++) {
  int16View[i] = i*100;
}
console.log(int16View);                 //Int16Array(4) [0, 100, 200, 300, 400, 500, 600, 700]

var int8View = new Int8Array(buffer);   //用一個chunk大小為8bits(1Bytes)的TypedArray來操作
console.log(int8View);                  //int8Array(8) [0, 0, 100, 0, -56, 0, 44, 1, -112, 1, -12, 1, 88, 2, -68, 2]

var int32View = new Int32Array(buffer); //用一個chunk大小為32bits(1Bytes)的TypedArray來操作
console.log(int32View);                 //Int32Array(4) [6553600, 19661000, 32768400, 45875800]
```
在有資料的ArrayBuffer中用不同的TypedArray可能會產生溢位等情況，例如chunk大小為16bits的話，就可以放2^16大的值，但chunk大小為8bits的話，就會有有放不下，需注意

![ArrayBuffer](https://github.com/ninolin/note/blob/master/JavaScript/images/ArrayBuffer.png)

## DataView

DataView也是操作Buffer的物件，比起TypedArray不需要固定資料型別，需是指定從那一個byte存取幾btyes的資料
```
var buffer = new ArrayBuffer(4);        //產生一個長度為4Bytes的二進位資料(值預設為0)
const dv = new DataView(buffer);        //產生一個DataView來操作buffer
dv.setInt8(3, 100);                     //在第3個bytes的地方放一個值
dv.getInt8(3);                          //100

var int8View = new Int8Array(buffer);
console.log(int8View);                  //Int8Array(4) [0, 0, 0, 100]
```
