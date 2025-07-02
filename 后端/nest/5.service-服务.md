# service 服务



### serivce 使用

#### 注册

在 nest 中使用 service 需要在 模块 的 **providers**（提供者）中注册：

```ts
import {Module} from 'nest/common'
import {TestService} from './test.service'

@Module({
    providers: [TestService]
})
export class AppModule {}
```



#### 使用

使用注册的 service, 在 controller 中声明

```ts
import {Controller} from 'nest/common'
import {testService} from './test.service'

@Controller
export class testController {
    constructor(private readonly testService: TestService) {}
}
```

**注意：** 类型 **TestService** 有两个作用：

1. 最为 key 去 module 查看该 service 是否注册
2. 最为 ts 类型



#### 自定义注册

一般service 直接在 module 中的 providers 中直接填写名称注册，但也可以使用如下方式注册：

```ts
import {Module} form 'nest/common'

@Module({
    providers:[
        {
            provider: 'myService',
            useValue: '123'
        }
    ]
    
})

// 也可以如下注册（简单方式注册
@Module({
    providers:['myervice'] // 字符串
    
})

export class AppModule {
    
}
```

- provider：servce 注册的名称，当为值时（userValue），需要使用 inject 注入

```ts
import {Controller, Inject} from 'nest/common'

@Controller()
export class TestControllrt {
    constructor(Inject('myService') provite readonly myService: string) {
        
    }
}
```



### service的值类型：

#### useValue

以value的方式注册 service，如上，使用时需要 **inject** 注入, 在简单方式注册时为字符串

#### userClass

以class的方式注册 service，使用无需注入

```ts
//注册
import {Module} form '@nestjs/common';
import {TestService} from 'test.service' 
@Module({
    providers:[
        {
            provider: 'TestService',
            useValue: TestService
        }
    ]
})

// 也可如下注册
@Module({
    providers:[TestService]
})

export class AppModule {
    
}



//使用
import {Controller, Inject} from '@nestjs/common';
import {TestService} from 'test.service' 
@Controller()
export class TestControllrt {
    constructor( provite readonly testService: TestService) {
        
    }
}
```



#### useFactory

以工程函数的方式注册 service，使用时需 Inject 注入，**工厂函数可以返回任意值，以及class 实例**

**注册**

```ts
//注册
import {Module} form '@nestjs/common';
import {TestService} from 'test.service' 
@Module({
    providers:[
        {
            provider: 'TestService',
            useFactory() {
                return new TestService
                // return 'aa'
            }
        }
    ]
})

// 使用
import {Controller, Inject} from '@nestjs/common';
import {TestService} from 'test.service' 
@Controller()
export class TestControllrt {
    constructor(@Inject('TestService') provite readonly testService: TestService // string Record<string, any>) {
        
    }
}
```

**useFactory 使用 inject**:  在注册自定义 service 中 可以使用 inject 注入其他 service, 注入的 service 会被注入成 useFactory 的参数，inject 接收 service 数组

```ts
//注册
import {Module} form '@nestjs/common';
import {TestService} from 'test.service' 
import {OtherService} from 'other.service'
@Module({
    providers:[
        {
            provider: 'TestService',
            inject:[OtherService]
            useFactory(service) {
        		console.log(service) // service 为 注入的 otherService
                return new TestService
                // return 'aa'
            }
        }
    ]
})

// 使用
import {Controller, Inject} from '@nestjs/common';
import {TestService} from 'test.service' 
@Controller()
export class TestControllrt {
    constructor(@Inject('TestService') provite readonly testService: TestService // string Record<string, any>) {
        
    }
}
```



### 共享Service

每个 Module 都有自己的作用域，每个Module之间无法互相调用其内部的service，如果要在一个模块中调用另一个模块中Service:

1. 在模块中导出service

   ```ts
   //注册
   import {Module} form '@nestjs/common';
   import {TestService} from 'test.service' 
   @Module({
       providers: [TestService],
       exports: [TestService] // 导出 service 供其他 Module 使用
   })
   export class AppModule {}
   ```

   

2. 引入这个模块

```ts
import {Module} form '@nestjs/common';
import {AppModule} from 'app.module'
@Module({
    import: [AppModule] // 导入，
})
export class OtherModule{}
```

即可在这个OtherModule内部使用 appmodule 中的 service

### 异步Service

service可以是异步的，也就是说 useFactory 可以返回一个promise：

```ts
import { Module } from '@nestjs/common';
import { AaController } from './aa.controller';
@Module({
  providers: [
    {
      provide: 'h',
      useFactory() {
        return new Promise((res) => {
          setTimeout(() => {
            res('1111111');
          }, 3000);
        });
      },
    },
  ],
  controllers: [AaController],
})
export class AaModule {}
```

### 装饰器

**@optional()**: 声明带默认参数的属性

```ts
import { Injectable, Optional } from '@nestjs/common';

@Injectable()
export class UserService {
  constructor(@Optional() private readonly userModel = {}) {}
}
```

