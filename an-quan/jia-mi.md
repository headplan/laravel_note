# 加密

### 知识点整理

```
1.Laravel的加密器使用OpenSSL来提供AES-256和AES-128加密
2.配置
config/app.php中设置key选项为32位随机字符串
php artisan key:generate 生成key
3.使用加密器
加密:encrypt
解密:decrypt
use Illuminate\Contracts\Encryption\DecryptException;
try {
    $decrypted = decrypt($encryptedValue);
} catch (DecryptException $e) {
    //
}

不进行序列化的加密
use Illuminate\Support\Facades\Crypt;
$encrypted = Crypt::encryptString('Hello world.');
$decrypted = Crypt::decryptString($encrypted);
```



