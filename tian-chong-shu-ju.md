# 填充数据

### 知识点整理

```
1.简介
    填充类都位于database/seeds目录.
    使用call方法在DatabaseSeeder类中来运行其他填充类.
    
2.编写填充器
    生成填充器
    php artisan make:seeder UserTableSeeder
    编辑run方法.
        可以使用查询构建器手动插入数据.
            public function run()
            {
                DB::table('users')->insert([
                    'name' => str_random(10),
                    'email' => str_random(10).'@gmail.com',
                    'password' => bcrypt('secret'),
                ]);
            }
        也可以使用 Eloquent 模型工厂
            public function run(){
                factory('App\User', 50)->create()->each(function($u) {
                    $u->posts()->save(factory('App\Post')->make());
                });
            }
            
        调用额外的填充器
        使用call方法执行额外的填充类,在DatabaseSeeder类中
        public function run(){
            $this->call(UserTableSeeder::class);
            $this->call(PostsTableSeeder::class);
            $this->call(CommentsTableSeeder::class);
        }
        
3.运行填充器
    php artisan db:seed
    php artisan db:seed --class=UserTableSeeder
    php artisan migrate:refresh --seed // 这样会完全重建数据库,再填充数据
```



