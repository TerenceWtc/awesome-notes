# Redis

## [Get Ready](https://github.com/TerenceWtc/documents/blob/master/Linux/Redis/Redis.md)

## Application.yml
```
spring:
  redis:
    host: 192.168.17.128
    database: 0
    port: 6379
    timeout: 1000
    password:
```

## Enable Cache
Add annotation `@EnableCaching` in `BackendApplication` which is the main entry.

## Configuration
```
@Configuration
@EnableCaching
public class RedisConfig {

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory redisConnectionFactory) {
        // initialize a RedisCacheWriter
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory);
        // jackson to json serializer will cause StackOverFlow exception
        // RedisSerializer<Object> jsonSerializer = new GenericJackson2JsonRedisSerializer();

        // use fastJson serializer
        RedisSerializer<Object> jsonSerializer = new GenericFastJsonRedisSerializer();
        RedisSerializationContext.SerializationPair<Object> pair = RedisSerializationContext.SerializationPair
                .fromSerializer(jsonSerializer);
        RedisCacheConfiguration defaultCacheConfig = RedisCacheConfiguration.defaultCacheConfig()
                .serializeValuesWith(pair);
        // this config does not work
        // defaultCacheConfig.entryTtl(Duration.ofSeconds(30));

        // set default expire time 1800s
        RedisCacheConfiguration configuration = defaultCacheConfig.entryTtl(Duration.ofSeconds(1800));
        // initialize RedisCacheManager
        return new RedisCacheManager(redisCacheWriter, configuration);
    }
}
```

## Annotations
### @Cacheable
Put the result into redis cache
Three optional parameters: `value`, `key`, `condition`
The defualt generate rule of key in redis is the combination of `value::key`

### @CachePut
Update the key

### @CacheEvict
Remove the key