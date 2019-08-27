#redis引入依赖
```$xslt
<dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>2.0.7.RELEASE</version>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-cache</artifactId>
                <version>2.0.7.RELEASE</version>
            </dependency>
```

#redis 配置文件
```$xslt
spring:
  redis:
    host: 127.0.0.1
    port: 6379
    password:
    database: 0
    lettuce:
      pool:
        max-active: 32
        max-wait: 300ms
        max-idle: 16
        min-idle: 8
```
#Redis核心配置类
```$xslt
@Component
@Configuration
@ConfigurationProperties("spring.cache.redis")
//@EnableCaching
public class RedisConfig extends CachingConfigurerSupport {
    private Duration timeToLive = Duration.ZERO;


    public void setTimeToLive(Duration timeToLive) {
        this.timeToLive = timeToLive;
    }


    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);

        // 配置序列化（解决乱码的问题）
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(timeToLive)
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();

        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }

    //RedisTemplate配置
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);
        //配置事务
        template.setEnableTransactionSupport(true);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }

}
```

# 代码式加缓存
```$xslt
@Resource
private RedisTemplate redisTemplate;

public void set() {
    redisTemplate.opsForValue.set("test","this is ops for value");
}

public Object get() {
    return redisTemplate.opsForValue.get("test");
}

//模糊批量操作 redis
public void evictUserById(int id) {
        try {

            redisTemplate.delete(RedisInterface.USER_REDIS_ID_KEY + "::" + id);
            Set keys1 = redisTemplate.keys(RedisInterface.COMBO_DAY_LIST_UPC_ID_KEY+"::*");
            Set keys2 = redisTemplate.keys(RedisInterface.COMBO_DAY_LIST_TEL_KEY + "::*");
            redisTemplate.delete(keys1);
            redisTemplate.delete(keys2);
        }catch (Exception e) {
            e.printStackTrace();
        }
    }
```

# 注解式加缓存

```$xslt
类级别注解
@CacheConfig(cacheNames="")
方法级别注解
@Cacheable()
@CacheEvict()
@Caching(
    evict{
        @CacheEvict()
    }
)
```
