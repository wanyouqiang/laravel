# eloquent_ORM 使用_

### all()---静态方法 查询所有记录


### find()---静态方法 根据id查询一条记录

### 聚合函数---静态方法
* count() 查询记录数量
* max() 查询最大值
* min() 查询最小值
* avg() 查询平均值
* sum() 查询总和

### Query Builder(查询构造器) 根据条件来检索需要的数据
* where()---静态方法

    `
        $cat = Cat::where('id', '=', 1)->first(); // 查询id为1的记录
        $cat = Cat::where('id', '=', 1)->where('gender', '=', 'Male')->first(); // 使用链式操作
    `

* first() 用来获取第一条记录(一般与where静态方法连用)

    `
        Cat::where('name', 'like', 'miao')->first();
    `

* get() 用来获取所有记录(一般与where()静态方法连用)

    `
        Cat::where('name', 'like', 'miao')->get();
    `

* take(3) 指定获取的记录数
    `
        Cat::where('name', 'like', 'miao')->take(3)->get();
    `

* skip(3) 指定获取记录的起始位置

     `
        Cat::where('name', 'like', 'miao')->take(3)->skip(3)->get();
    `

* orderBy('field', 'desc/asc') 根据指定的列对记录进行排序

    `
        Cat::orderBy('id', 'desc')->get();
        Cat::where('name', 'like', 'miao')->orderBy('id', 'desc')->get();
    `
### 保存数据
* save() 持续化到数据库
    
    `
        $user = User::find(1);
        $user->name = 'mike';
        $user->save();
    `
### 删除记录
* delete()

    `
        $user = User::find(1);
        $user->delete();
    `

* destroy() 静态方法，传入ID直接进行删除操作

    `
        $flag = User::destory(1);
    `
* 软删除
    
    `
        // 在Model中引入SoftDeletes特性
        protected $dates = ['deleted_at'];

        // 控制器
        $user = User::find(1);
        $user->delete();

        // 检索已经删除的记录
        User::withTrashed()->get(); // 混合模式
        User::onlyTrashed()->get(); // 只检索已经删除的记录

        User::onlyTrashed()->where('id', '=', 1)->restore(); // 恢复已经删除的记录
    `

### Relation(模型关联)
* 一对一（hasOne、belongsTo）
    `
        // User.php
        public function profile()
        {
            return $this->hasOne('App\Http\Models\Profile', 'user_id');
        }

        // Profile.php
        public function user()
        {
            return $this->belongsTo('App\Http\Models\User');
        }
        // 一对一新增
        $user = new User();
        $user->name = 'mike';

        $profile = new Profile();
        $profile->email = 'abc@qq.com';

        $user->profile()->save($profile); // 使用save()方法进行一对一关联新增
    `

* 一对多 (hasMany、belongsTo)

    `
        // User.php
        public function cats()
        {
            return $this->hasMany('App\Http\Models\Cat', 'user_id');
        }

        // Cat.php
        public function user()
        {
            return $this->belongsTo('App\Http\Models\User');
        }

        // 一对多新增 saveMany()方法
        /*
            给用户id为1新增一只猫
            $cat = new Cat();
            $cat->name = 'miao';
            $user = User::find(2);
            $user->cats()->saveMany([$cat]);

        */
    `

* 多对多 (belongsToMany)

    `
        // User.php
        public function roles()
        {
            return $this->belongsToMany('App\Http\Models\Role', 'user_role', 'user_id', 'role_id');
        }

        // Role.php
        public function users()
        {
            return $this->belongsToMany('App\Http\Models\User', 'user_role', 'role_id', 'user_id');
        }

        // 多对多新增关联
        /*
            权限表中有分别为1和2的两个权限，新增一个用户，并赋予该用户两个权限

        */
        $user = new User();
        $user->name = "mike";
        $user->save();
        $user->roles()->attach([1, 2]); // attach附加，也可以使用sync()方法

        // 撤消用户的role_id = 2的权限
        $user = User::find(5);
        $user->roles()->detach([2]); // detach分离
    `
