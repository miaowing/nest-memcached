<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo_text.svg" width="320" alt="Nest Logo" /></a>
</p>

## Description

This is a [Memcached](http://memcached.org/) module for [Nest](https://github.com/nestjs/nest).

## Installation

```bash
$ npm i --save nest-memcached memcached
```

## Quick Start

#### Import Module

```typescript
import { Module } from '@nestjs/common';
import { MemcachedModule } from 'nest-memcached';

@Module({
  imports: [
      MemcachedModule.register({
        uri: [
           '192.168.0.102:11211',
           '192.168.0.103:11211',
           '192.168.0.104:11211'
        ],
        retries: 3
      })
  ],
})
export class ApplicationModule {}
```

If you use [nest-boot](https://github.com/miaowing/nest-boot) module.

```typescript
import { Module } from '@nestjs/common';
import { MemcachedModule } from 'nest-memcached';
import { BootModule, BOOT_ADAPTER } from 'nest-boot';

@Module({
  imports: [
      BootModule.register(__dirname, 'bootstrap.yml'),
      MemcachedModule.register({adapter: BOOT_ADAPTER})
  ],
})
export class ApplicationModule {}
```

##### bootstrap.yml

```yaml
memcached:
  uri: ['192.168.0.102:11211', '192.168.0.103:11211', '192.168.0.104:11211'],
  retries: 3
```

If you use [nest-consul-config](https://github.com/miaowing/nest-consul-config) module.

```typescript
import { Module } from '@nestjs/common';
import { MemcachedModule } from 'nest-memcached';
import { ConsulModule, CONSUL_ADAPTER } from 'nest-consul';
import { ConsulConfigModule } from 'nest-consul-config';

@Module({
  imports: [
      ConsulModule.register({/* ignore */}),
      ConsulConfigModule.register({/* ignore */}),
      MemcachedModule.register({adapter: CONSUL_ADAPTER})
  ],
})
export class ApplicationModule {}
```

##### config in consul kv

```yaml
memcached:
  uri: ['192.168.0.102:11211', '192.168.0.103:11211', '192.168.0.104:11211'],
  retries: 3
```

#### Memcached Client Injection

```typescript
import { Component } from '@nestjs/common';
import { InjectMemcachedClient, Memcached } from 'nest-memcached';

@Component()
export class TestService {
  constructor(@InjectMemcachedClient() private readonly memClient: Memcached) {}

  async addValue(key: string, value: string): void {
      await this.memClient.add(key, value);
  }
  
  async deleteValue(key: string): void {
      await this.memClient.del(key);
  }
}
```

## Support

Nest is an MIT-licensed open source project. It can grow thanks to the sponsors and support by the amazing backers. If you'd like to join them, please [read more here](https://opencollective.com/nest).

## Stay in touch

- Author - [Miaowing](https://github.com/miaowing)

## License

  Nest is [MIT licensed](LICENSE).
