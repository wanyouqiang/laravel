# Laravel 中间件（Middleware）
### 中间件的作用
中间件提供了一个方便的机制用于*过滤*Http请求到具体的应用

### 创建中间件
* 使用命令 php artisan make:middleware CheckAge
* 手动创建，在app/Http/Middleware中进行创建

### 注册中间件(Kernel.php)
* 全局注册
```
    全局注册适用于所有的路由，在Kernel.php的Kernel类中，找到middleware成员属性，将自定义的中间件全路径添加既可

```
* 路由中间件
```
    如果希望某一中间件适用于特定路由，首先要在Kernel.php的Kernel类中，找到routeMiddleware成员属性，将自定义的中间件全路径添加即可，其次，在路由中使用middleware方法进行配置
```

### 在路由中使用中间件
* 单一配置

```
    Route::get('/login', 'IndexController@login')->middleware('CheckAge', 'another');

    use App\Http\Middleware\CheckAge;
    Route::get('/login', 'IndexController@login')->middleware(CheckAge::class);
```

* 分组配置（同一个中间件适用于多个路由）

```
    Route::middleware(['first', 'second'])->group(function () {
        Route::get('/', function () {

        });

        Route::get('/index', function () {

        });
    });

    Route::group(['middleware' => ['web']], function () {
        Route::get();
        Route::post();
    });
```