---
layout: post
title:  "Spring Cache"
author: 악어새62
categories: [ TIL, Backend]
image: assets/images/1.jpg
tags: []
---
## 개요

데이터베이스 쿼리 튜닝에서 레디스로 그리고 스프링에 적용하는 방법까지 이어져서  
사용할 수 있는 애노테이션들을 정리해본다.

## Spring Cache

Spring Framework는 캐싱 추상화(Cache Abstraction)를 제공해서(2011년 릴리즈), 로직에서 캐시 로직을 분리하고 선언적으로 제어할 수 있게 해준다.
대표적으로 사용하는 애노테이션은 다음과 같다.

### @Cacheable

@Cacheable은 캐시에서 조회하거나 새로 저장하는 애노테이션으로 사용은 다음과 같다.
```java
@Cacheable(value = "products", key = "#id")
public Product getProduct(Long id) {
    return productRepository.findById(id).orElseThrow();
}
```

1. 메서드 호출 시 먼저 캐시에서 key=#id로 데이터를 찾는다.
2. 캐시에 있으면 메서드를 실행하지 않고 캐시값만 반환한다.(Cache Hit)
3. 캐시에 없다면 메서드 실행 후 결과를 캐시에 저장한다.(Cache Miss -> Store)

주요 속성은 다음의 표와 같다.
|속성|설명|
|--|--|
| value | 캐시이름(여러 개 지정 가능) |
| key | 캐시 키 SpEL표현식(#id, #root.methodName 등) |
| condition | 캐싱할 조건(#id > 10 등) |
| unless | 캐싱하지 않을 조건(#result == null 등) |
| sync | true면 동일 키의 병렬 호출 방지(한 번만 실행 후 캐싱) |


### @CachePut

항상 메서드 실행 + 캐시 갱신의 기능의 애노테이션이다.

위의 @Cacheable과 달리 항상 메서드를 실행한다.  
실행 결과를 캐시에 저장한다.(덮어씀)

```java
@CachePut(value = "products", key = "#product.id")
public Product updateProduct(Product product) {
    return productRepository.save(product);
}
```
주로 DB업테이트 후 캐시도 동기화 해야할 때 사용한다.

## @CacheEvict

캐시 삭제 애노테이션으로 캐시에서 지정한 키를 삭제한다.
```java
@CacheEvict(value = "products", key = "#id")
public void deleteProduct(Long id) {
    productRepository.deleteById(id);
}
```
캐시에서 지정한 키를 삭제하고 allEntires = true일 경우 캐시 전체를 삭제할 수 있다.

|속성|설명|
|--|--|
| key | 삭제할 캐시 키 |
| allEntries | true → 캐시 이름 전체 삭제 |
| beforeInvocation | true → 메서드 실행 전에 삭제 (기본은 false, 즉 실행 후 삭제) |

### @Caching

여러 캐시 애노테이션을 묶을 수 있다.
```java
@Caching(
  cacheable = { @Cacheable(value = "productCache", key = "#id") },
  put = { @CachePut(value = "productCache", key = "#result.id") },
  evict = { @CacheEvict(value = "oldCache", key = "#id") }
)
public Product getOrUpdateProduct(Long id) {
    return productRepository.findById(id).orElseThrow();
}
```

### @CacheConfig
공통 캐시 설정 애노테이션으로 클래스 레벨에서 공통 캐시 이름이나 KeyGenerator CacheManager 지정 가능하다.  
이 애노테이션을 사용하면 매서드 단위 애노테이션의 중복을 제거할 수 있다.
```java
@CacheConfig(cacheNames = "products")
@Service
public class ProductService {
    @Cacheable(key = "#id")
    public Product getProduct(Long id) { ... }
}
```

## 추상화

해당 애노테이션은 JDBC와 같이 스프링에서 인터페이스를 작성하고 그 명세를 구체적인 구현체(Redis, Caffenie)가 구현하여 동작한다.  
예를 들어 스프링에서는 `org.springframework.cache.Cache, CacheManager` 인터페이스를 제공하고 구체적인 구현체는 `RedisCacheManager, EhCacheCacheManager, CaffeineCacheManager`등이 있다.  
이렇게 구체적인 구현체가 달라지더라도 위에서 설명한 애노테이션을 통해 일괄된 접근 방식으로 접근할 수 있다.
```
@Cacheable
   ↓
CacheInterceptor
   ↓
CacheManager
   ↓
Cache (실제 캐시 구현체)
   ↓
Redis / Caffeine / Ehcache ...
```

예를 들어 다음과 같이 코드를 작성했다고 해보자.
```java
@Cacheable(value = "user", key = "#id")
public User getUser(Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

위의 코드를 실행하면 내부적으로
1. CacheInterceptor가 매서드 호출을 가로챈다(AOP)
2. CacheManager에게 user캐시에서 id키 찾기를 요청한다.
3. 등록된 구현체가 Redis라면 RedisCacheManager가 Redis에 접근한다.
4. 캐시가 없으면 DB에서 조회 후 결과를 해당 캐시에 저장한다.


## 캐시의 활용

주로 데이터베이스 부하 완화에 사용한다.

1. 데이터베이스 부하 완화: 가장 쉽게 떠올릴 수 있는 것은 데이터베이스에서 조회한 결과를 캐싱하여 이후 요청에는 이 캐시의 값을 대신 사용하여 데이터베이스 부하를 줄이는 방법이다.
2. JWT인증/인가 최적화: 로그인 시 발급한 토큰을 Redis에 저장하여 읽고 씀으로 데이터베이스 접근을 줄일 수 있다.
3. 좋아요, 조회수 기능 고도화: INCR 연산은 숫자형 값을 원자적으로 1 증가시킨다. 따라서 여러 요청이 들어와도 Redis 내부에서 Race Condition없이 안전하게 처리가 가능하다.
4. 랭킹 처리
```java
@Service
@RequiredArgsConstructor
public class ViewCountService {
    private final StringRedisTemplate redisTemplate;

    public Long increaseViewCount(Long postId) {
        String key = "post:" + postId + ":view";
        // INCR 명령 실행
        return redisTemplate.opsForValue().increment(key);
    }

    public Long getViewCount(Long postId) {
        String key = "post:" + postId + ":view";
        String count = redisTemplate.opsForValue().get(key);
        return count != null ? Long.parseLong(count) : 0L;
    }
}
```
위의 코드와 같이 redisTemplate라는 저수준 API를 사용하여 INCR명령을 실행 시킬 수 있다.

또한 ZINCRBY 명령어를 이용하여 정렬된 랭킹 관리가 가능하다.  
예를 들어 ZEST 자료구조의 특정 멤버 점수를 N만큼 증가시키고 점수는 자동으로 정렬되어 랭킹을 계산할 수 있다.
```cmd
ZINCRBY rank 1 user:junhyuk     # user:junhyuk 점수 +1
ZINCRBY rank 2 user:hana        # user:hana 점수 +2
ZINCRBY rank 3 user:lee         # user:lee 점수 +3

ZRANGE rank 0 -1 WITHSCORES     # 오름차순
ZREVRANGE rank 0 -1 WITHSCORES  # 내림차순 (랭킹 순위)

1) "user:lee"   3
2) "user:hana"  2
3) "user:junhyuk" 1
```
자바 코드로 나타내보면
```java
@Service
@RequiredArgsConstructor
public class RankingService {

    private final StringRedisTemplate redisTemplate;
    private static final String KEY = "post:ranking";

    public void increaseScore(String postId, double score) {
        redisTemplate.opsForZSet().incrementScore(KEY, postId, score);
    }

    public Set<ZSetOperations.TypedTuple<String>> getTopRank(int count) {
        // 점수 높은 순으로 상위 N개 조회
        return redisTemplate.opsForZSet().reverseRangeWithScores(KEY, 0, count - 1);
    }
}
```

## 레디스 구조적 특징

레디스는 단일 스레드 이벤트 루프 모델로 작동한다.  
즉, 한 번에 하나의 명령만 실행되는 것이다.  
그래서 여러 클라이언트가 동시에 INCR 명령을 보내더라도 하나의 요청을 처리하는 동안 큐에 명령을 보관한다.

이렇게 해서 A명령이 끝난 뒤 B 명령이 수행된다.

INCR은 내부적으로 아래의 단계를 한 번에 수행한다.
1. 키의 값을 읽는다.
2. 정수로 변환한다.
3. +1 증가시킨다.
4. 다시 저장한다.

이 과정을 하나의 명령 단위로 원자적으로 수행한다.

Redis는 메모리 기반 저장으로 모든 데이터가 RAM에 저장된다. 이로 인해 디스크 I/O가 없어서 DB보다 수십~수백 배 빠르다.  
그리고 String, List, Set, Sorted Set, Hash 등 자료구조별로 최적화되어있어 대부분의 연산이 O(1) 혹은 O(logN)의 시간복잡도를 가진다.  
INCR -> O(1)
ZINCRBY -> O(logN)

