## [완전탐색_1](./완전탐색_1.md) 사용한 메서드 , 컬렉션 REVIEW

### REVIEW 

```java
// - - - - -  Hash Map , LIST
  Map<Integer, Integer> map = new HashMap<>();

  map.put(1,0);
  map.put(2,0);
  map.put(3,0);

  value = map.get(key)+1;

  List<Integer> valueList = new ArrayList<>(map.values());
  List<Integer> returnList = new ArrayList<>();

    for(int k : map.keySet()){
        if(max == map.get(k)){
            returnList.add(k); 
        }
    }

  int[] result = new int[returnList.size()];

  //- - - - - Collections
  int max = Collections.max(valueList);

  
  // - - - - - - 2차원 배열
  int[][] supoArr = {
      {1,2,3,4,5},
      {2,1,2,3,2,4,2,5},
      {3,3,1,1,2,2,4,4,5,5}
  };
      
```