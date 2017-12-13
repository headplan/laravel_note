# 单元测试

```
public function setUp()
{
    // first include all the normal setUp operations
    parent::setUp();

    // now re-register all the roles and permissions
    $this->app->make(\Spatie\Permission\PermissionRegistrar::class)->registerPermissions();
}
```



