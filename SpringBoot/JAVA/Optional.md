### Optional<> 객체

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

[참고](http://www.tcpschool.com/java/java_stream_optional)