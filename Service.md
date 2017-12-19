# Service Container & ServiceProvider & Facade

### Service Container(服务容器)
装载服务的容器，相当于人的大脑。服务容器是在bootstrap目录中的app.php文件中生成，并在入口文件中进行引入。

### Service Provider(服务提供者)
向服务容器提供服务的注册与绑定，使用命令php artisan make:provider DemoProvider创建Service Provider。注册一个服务到服务容器需要三步：
* 创建ServiceProvider
* 在register方法中进行绑定
* 在config/app.php文件中进行注册

### Facade(门面)
Facade 提供了一个静态的接口访问服务容器中的服务，所有的Facade都要继承自Illuminate\Support\Facades。为指定服务创建一个Facade的步骤：
* 新建一个自定义Facade类继承自Illuminate\Support\Facades\Facade类
* 重写父类中的getFacadeAccessor方法返回服务在注册时的key值即可。
* 在config/app.php文件中为Facade指定别名。