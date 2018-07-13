## Swoole使用

1) 不可以通过设置set_exception_handler捕获全局异常,
   但是可以通过设置register_shutdown_function在未捕获异常或错误发生时,执行一些必要的操作.