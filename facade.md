# Facade in laravel

### Introduction(简介)
*Facades 提供了一个静态的接口服务于应用的service container相应的类，Laravel本身带有很多的Facades,你可以直接使用而不需要深入了解它*

### 创建POJO
    ``
    	class User
	{
	    public function showInfo()
	    {
	    	echo 'name:youqiang';
	    }
	}
    ``

### 创建Facades
*在app目录下新建Facades目录*

    ``
    	namespace App\Facades;

	use Illuminate\Support\Facades\Facade;

	class FacadesDemo extends Facade
	{
	    protected static function getFacadeAccessor()
	    {
	        return 'test';
	    }
	}
    
    ``
 
### 创建service provider
*使用artisan命令：php artisan make:provider MyServiceProvider*

    ``
    	public function register()
	{
	    App::bind('test', function () {
	        return new \App\Test\User;
	    });
	}
    ``
### 注册service provider 与 facades
*在app.php中相应位置注册*

### 使用Facades

    ``
    	Route::get('/', function () {
             echo UserFacades::showInfo();	    
	})
    ``
