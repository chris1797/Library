# Redis 기본 자료구조

<br>

## 1. String
가장 기본적인 데이터 타입으로, 단일 키에 문자열 값을 저장하는 형태이다.
- 예시 명령어 : SET, GET, INCR, DECR, APPEND
- 용도 : 캐싱, 카운터, 간단한 문자열 데이터 저장

```bash
SET user:1000 "Alice"
GET user:1000            # "Alice"
INCR views               # views 카운트 1 증가
```

<br/>

## 2. List
Redis 리스트는 삽입 순서가 유지되는 링크드 리스트 형태로, 값을 양방향(왼쪽과 오른쪽)에서 삽입하거나 삭제할 수 있다.
- 예시 명령어 : LPUSH, RPUSH, LPOP, RPOP, LRANGE
- 용도 : 대기열, 최근 활동 로그, 메세지 큐
  
```bash
LPUSH tasks "task1"      # 왼쪽에 "task1" 추가
RPUSH tasks "task2"      # 오른쪽에 "task2" 추가
LPOP tasks               # 왼쪽에서 "task1" 제거
```

<br/>

## 3. Set
Set은 중복되지 않는 유일한 값들의 집합을 나타내며, 원소의 순서는 보장되지 않는다.
- 예시 명령어 : SADD, SREM, SMEMBERS, SISMEMBER
- 용도 : 태그 저장, 중복 없는 값 저장, 교집합/합집합 계산

```bash
SADD tags "java"         # "java" 태그 추가
SADD tags "redis"        # "redis" 태그 추가
SADD tags "java"         # 중복이므로 추가되지 않음
SMEMBERS tags            # "java", "redis"
```

<br/>

## 4. Hash
Hash는 키-필드-값 구조로, 객체처럼 여러 필드를 저장할 수 있는 데이터 타입이다. 각 필드는 독립적으로 접근할 수 있다.
- 예시 명령어 : HSET, HGET, HGETALL, HDEL
- 용도 : 객체 데이터 저장 (ex: 사용자 프로필, 제품 정보)

```bash
HSET user:1000 name "Alice"
HSET user:1000 age 30
HGET user:1000 name       # "Alice"
HGETALL user:1000         # 모든 필드 조회
```
