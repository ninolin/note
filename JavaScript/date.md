# JS Date 使用介紹

## 使用 Date 物件

JS 的 Date 物件存在不少的地方需要注意，特別是直接帶入日期字串進去的時侯
*   new Date(): 回傳目前的時間 => ex: Tue Jun 25 2019 17:15:40 GMT+0800 (台北標準時間)
*   new Date(value): 回傳 value 的時間 => value 為單位為毫秒的timestamp
*   new Date(dateString): 回傳 dateString 的時間 => 要能被Date.parse()的日期字串格式(不同的瀏覽器的結果會有差異，不建議使用)
*   new Date(year, month[, day[, hour[, minutes[, seconds[, milliseconds]]]]]): 回傳帶入參數的時間

注意，出來的日期會是當地的時間

## 使用 dateString 的注意點

日期字串常出現在excel等外部資料匯入時發生，下面給4個範例在2個瀏覽器下的結果，Safari跟Chrome有一個日期格式結果不同，特別的地方是'2019-01-05'格式跟其它結果不同，因為 JS 的日期是用 ISO8601 格式(YYYY-MM-DDTHH:mm:ss.sssZ)，所以'2019-01-05'會自動被轉成當地時間，像台灣就多了8小時，因此日期字串是2019-9-30這種格式的話，到2019-10-10就自動多了8小時了。

Chrome 75.0.3770.100
```
new Date('2019-1-5')
Sat Jan 05 2019 00:00:00 GMT+0800 (台北標準時間)
new Date('2019-01-05')
Sat Jan 05 2019 08:00:00 GMT+0800 (台北標準時間)
new Date('2019/1/5')
Sat Jan 05 2019 00:00:00 GMT+0800 (台北標準時間)
new Date('2019/01/05')
Sat Jan 05 2019 00:00:00 GMT+0800 (台北標準時間)
```

Safari 12.1
```
> new Date('2019-1-5')
< Invalid Date
> new Date('2019-01-05')
< Sat Jan 05 2019 08:00:00 GMT+0800 (CST)
> new Date('2019/1/5')
< Sat Jan 05 2019 00:00:00 GMT+0800 (CST)
> new Date('2019/01/05')
< Sat Jan 05 2019 00:00:00 GMT+0800 (CST)
```
