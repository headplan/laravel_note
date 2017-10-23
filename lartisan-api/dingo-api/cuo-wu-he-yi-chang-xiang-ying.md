# 错误和异常响应

在构建API的时候处理错误是一件痛苦的事儿 , 在Dingo API中 , 你不需要手动构建错误响应 , 只需要抛出一个继承自`Symfony\Component\HttpKernel\Exception\HttpException`的异常 , API会自动为你处理这个响应 . 

