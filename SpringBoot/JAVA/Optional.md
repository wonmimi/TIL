### Optional< T > 객체
null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE(NullPointerException)가 발생하지 않도록 도와준다

ex) 소스 코드 

```java
@Override
    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        // pk 기반이 아닌 데이터 조회는 jpql 작성
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();

        return result.stream().findAny();

    }

```

[참고_1](http://www.tcpschool.com/java/java_stream_optional) / 
[참고_2](https://mangkyu.tistory.com/70)