## HashSet

: 내부적으로 HashMap을 이용해 만들어진 Set으로, `해싱(Hashing)`을 이용해서 구현

- Set 인터페이스를 구현한 `가장 대표적인 컬렉션`
- Set 인터페이스의 특징대로 `중복 허용 X`
- 저장순서를 유지하지 않음 → 순서를 유지하려면 `LinkedHashSet`

|             Method              |                   Explanation                    |
| :-----------------------------: | :----------------------------------------------: |
|      boolean add(Object o)      |                 새로운 객체 삽입                 |
|  boolean addAll(Collection c)   |          주어진 컬렉션의 모든 객체 삽입          |
|          void clear()           |              저장된 모든 객체 삭제               |
|   boolean contains(Object o)    |        지정된 객체를 포함하고 있는지 리턴        |
| boolean contains(Collection c)  | 지정된 컬렉션의 모든 객체를 포함하고 있는지 리턴 |
|        boolean isEmpty()        |            HashSet이 비어있는지 리턴             |
|    boolean remove(Object o)     |                 지정된 객체 삭제                 |
| boolean removeAll(Collection c) |          지정된 컬렉션의 모든 객체 삭제          |
| boolean retainAll(Collection c) |  지정된 컬렉션의 모든 객체를 제외한 나머지 삭제  |
|           int size()            |         HashSet에 저장된 객체의 수 리턴          |
