### nestjs-redis使用

#### 安装以下依赖：

- ioredis
- @nestjs-modules/ioredis

```ts
npm i ioredis @nestjs/modules/ioredis -S
```

#### 注册 RedisModule

可以在app.module中注册 RedisModule，也可以写在单独的module中，这里我写在单独的 module中：redis.module，并在 app.module 中引入它：

```ts
// redis.module.ts
import { Module } from '@nestjs/common';
import { RedisModule } from '@nestjs-modules/ioredis';
import { configEnum } from '@/enum/config.enum';
import { ConfigService } from '@nestjs/config';
@Module({
  imports: [
    RedisModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (config: ConfigService) => {
        return {
          type: 'single',
          url: config.get<string>(configEnum.REDIS_HOST),
          options: {
            password: config.get<string>(configEnum.REDIS_PASSWORD),
          },
        };
      },
    }),
  ],
})
export class RedisModules {}

// app.module.ts
Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: [`.env.${process.env.NODE_ENV}`], // 根据环境读取相应的配置
    }),
    RedisModules, // redis
  ],
```

#### 使用

在 service 中注册 redis 后便可直接使用

```ts
// app.service.ts
import { Injectable } from '@nestjs/common';
import { Redis } from 'ioredis';
import { InjectRedis } from '@nestjs-modules/ioredis';

@Injectable()
export class AppService {
  constructor(@InjectRedis() private readonly redis: Redis) {}

  async getHello(): Promise<string> {
    await this.redis.set('token', '123');
    return 'Hello World!';
  }
}

```

