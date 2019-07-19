# 在 Laravel 中自訂 Artisan 指令列

在使用Laravel(5.7版)開發時，可以用Artisan指令來開發一些指令

可以使用下列指令來列出目前全部可以用的Artisan指令
```
php artisan list
```
可以使用下列指令來列出指定的Artisan指令說明
```
php artisan help route:clear
```

## 建立一個指令
可以手動將指令的執行程式放在 app/Console/commands 中，或是使用下列指令來建立一個新的指令
```
php artisan make:command SendMail
```

### 指令程式的結構
在使用指令建出來的指令程式有一定的結構，主要由名稱(signature)、說明(description)和handle函式組成，下面為剛剛的指令建出來的指令程式結構
```
<?php
namespace App\Console\Commands;
use Illuminate\Console\Command;

class SendMail extends Command
{
    protected $signature = 'command:name';
    protected $description = 'Command description';

    public function __construct()
    {
        parent::__construct();
    }

    public function handle()
    {
        //
    }
}
```

### 指令的名稱(signature)和說明(description)
signature是下指令時要下的名稱，如果我的signature值為'email:send'，那執行指令的指令如下
```
php artisan email:send
```
description是使用list和help時會出現的說明

### 指令傳入參數
使用指令時若需要傳參數進去，就可以在signature加上

```
protected $signature = 'email:send {user}';     //帶一個必填參數
protected $signature = 'email:send {user?}';    //帶一個選填參數
protected $signature = 'email:send {user=1}';   //帶一個有預設值的選填參數
```

在可以在指令上加上option
```
protected $signature = 'email:send {--queue}';      //帶一個option，有帶的的話值為1
protected $signature = 'email:send {--queue=}';     //帶的option必須要有值
protected $signature = 'email:send {--queue=hi}';   //帶的option有預設值
```

傳入參數的說明可以放在description

```
protected $signature = 'email:send {user : 使用者的 ID } {--queue= : 這個工作是否該進入隊列}';
```
### 取得傳入的參數

取得指定的參數或選項
```
public function handle()
{
    $userId = $this->argument('user');
    $queueName = $this->option('queue');
}
```
