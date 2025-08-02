# app相关api

- setGlobalPrefix()：设置请求接口的前缀
- get():获取module实例，如果已注册则返回实例，否则返回error
- enableCors(): 开启跨域
- enableVersioning(options): 设置版本

  - type: 版本类型，具体可以参照官网
  - defaultVersion: 默认版本号

  > 可以在 Controller 中使用 @version 装饰器来指定版本号