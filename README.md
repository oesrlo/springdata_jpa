# Data JPA 공부


##  JPA + QueryDSL

1. JPA 객체 사용

```java
 JPAQuery<Product> query = new JPAQuery(entityManager);
        QProduct qProduct = QProduct.product;

        List<Product> productList = query
                .from(qProduct)
                .where(qProduct.name.eq("펜"))
                .orderBy(qProduct.price.asc())
                .fetch();    
```

2. JPA Factory 사용

```java
  JPAQueryFactory jpaQueryFactory = new JPAQueryFactory(entityManager);
        QProduct qProduct = QProduct.product;

        List<Product> productList = jpaQueryFactory.selectFrom(qProduct)    // JPA Query 와 다르게 select 절부터 작성  
                .where(qProduct.name.eq("펜"))
                .orderBy(qProduct.price.asc())
                .fetch();
    
 // config 파일에서 JPAQueryFactory 객체를 @Bean 객체로 등록시 JPA 초기화할 필요 x   
```


- 전체 칼럼 조회가 아니라 일부만 조회하고 싶다면 select() 와 form() 메서드 구분해서 사용  
**select 대상이 하나인 경우**
```java
 List<String> productList = jpaQueryFactory
                .select(qProduct.name)
                .from(qProduct)
                .where(qProduct.name.eq("펜"))
                .orderBy(qProduct.price.asc())
                .fetch();
```
**조회 대상이 여러 개 인 경우:   , 사용하고 Tuple로 리턴값 지정**

```java
List<Tuple> tupleList = jpaQueryFactory
                .select(qProduct.name, qProduct.price)
                .from(qProduct)
                .where(qProduct.name.eq("펜"))
                .orderBy(qProduct.price.asc())
                .fetch();
```


---

+ 반환 메서드

```java
List<T> fech()          // 조회 결과를 리스트로 반환 
T fechOne               // 한건의 조회 결과 반환
T fechFrist()           // 여러 건의 결과 중 1 건 반환
Long fetchCount()       // 조회 결과 개수 반환
QueryResult<T> fetchResults()   //결과 리스트와 개수 포함한 QueryResults 를 반환
```
---



