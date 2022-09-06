## Iterator와 ListIterator

&nbsp; Iterator, ListIterator, 그리고 (다루진 않을거지만,) Enumeration은 모두 컬렉션에 저장된 요소를 접근할 때 사용되는 인터페이스다. Enumeration은 Iterator의 구 버전이고, ListIterator는 Iterator의 기능을 향상 시킨 것이다.

<br>

## Iterator

&nbsp; Collection Framework에서는 컬렉션에 저장된 요소들을 `읽어오는 방법을 표준화`하였다. Iterator 인터페이스는 Collection에 저장된 각 요소에 접근하는 기능을 가진 인터페이스이고, Collection 인터페이스에는 Iterator 인스턴스를 반환하는 `iterator()` 메서드가 정의돼 있다.

```java
public interface Iterator {
    boolean hasNext();
    Object next();
    void remove();
}

public interface Collection {
    ...
    public Iterator iterator();
    ...
}
```

&nbsp; `iterator()` 메서드는 Collection 인터페이스에 정의된 메서드이므로, List와 Set도 포함한다. Collection 클래스에 대해 iterator( )를 호출하여 `Iterator 객체`를 얻은 다음 반복문, 주로 while문을 사용해서 Collection 클래스의 요소들을 읽어온다.

<table align="center" border="3" data-ke-align="alignLeft" data-ke-style="style12">
    <tbody>
        <tr style="height: 20px;">
            <td style="width: 34.0067%; height: 20px;">Method</td>
            <td style="width: 65.9933%; height: 20px;">Explanation</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 34.0067%; height: 20px;">boolean hasNext( )</td>
            <td style="width: 65.9933%; height: 20px;">읽어 올 요소가 남아있는지 확인</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 34.0067%; height: 20px;">Object next( )</td>
            <td style="width: 65.9933%; height: 20px;">다음 요소를 읽음</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 34.0067%; height: 20px;">void remove( )</td>
            <td style="width: 65.9933%; height: 20px;">next( )로 읽어 온 요소 삭제 (선택적 기능)</td>
        </tr>
    </tbody>
</table>

&nbsp; 그럼 아래 코드를 보면서 어떻게 사용하는지 알아보자.

```java
// List
ArrayList list = new ArrayList();
Iterator iter = list.iterator();

while(iter.hasNext()){
    System.out.println(iter.next());
}

// Set
HashMap map = new HashMap();
Iterator iter = map.keySet().iterator(); // 또는 entrySet().iterator()

while(iter.hasNext()){
    String key = iter.next();
    String value = map.get(key);
    System.out.println(key + " : " + value);
}
```

<br>

## ListIterator 생성자

&nbsp; ListIterator는 Iterator를 상속받아 기능을 추가한 것이다. 컬렉션의 요소에 접근할 때 Iterator는 단 방향으로만 이동할 수 있지만, ListIterator는 양방향으로의 이동이 가능하다. 다만, 이름과 같이 List 인터페이스를 구현한 컬렉션에서만 사용 가능하다.</p>

<table align="center" border="3" data-ke-align="alignLeft" data-ke-style="style12">
    <tbody>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">Method</td>
            <td style="width: 71.1628%; height: 20px;">Explanation</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">void add(Object o)</td>
            <td style="width: 71.1628%; height: 20px;">컬렉션에 새로운 객체 추가&nbsp; (선택적 기능)</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">boolean hasNext( )</td>
            <td style="width: 71.1628%; height: 20px;">읽어 올 다음 요소가 남아있는지 확인</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">boolean hasPrevious( )</td>
            <td style="width: 71.1628%; height: 20px;">읽어 올 이전 요소가 남아있는지 확인</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">Object next( )</td>
            <td style="width: 71.1628%; height: 20px;">다음 요소를 읽어 옴</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">Object previous( )</td>
            <td style="width: 71.1628%; height: 20px;">이전 요소를 읽어 옴</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">int nextIndex( )</td>
            <td style="width: 71.1628%; height: 20px;">다음 요소의 index 반환</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">int previousIndex( )</td>
            <td style="width: 71.1628%; height: 20px;">이전 요소의 index 반환</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">void remove( )</td>
            <td style="width: 71.1628%; height: 20px;">next( ) 또는 previous( )로 읽어 온 요소 삭제 (선택적 기능)</td>
        </tr>
        <tr style="height: 20px;">
            <td style="width: 28.8372%; height: 20px;">void set(Object o)</td>
            <td style="width: 71.1628%; height: 20px;">next( ) 또는 previous( )로 읽어 온 요소를 지정된 객체로 변경</td>
        </tr>
    </tbody>
</table>

&nbsp; 그럼 코드와 함께 이해해보자.

```java
import java.util.*;

class ListIteratorEx {
    public static void main(String[] args) {
        ArrayList arrList = new ArrayList();

        for(int i=1 ; i<=5 ; i++)
            arrList.add(i);

        ListIterator listIter = arrList.listIterator();

        // 순방향 출력 → 12345
        while(listIter.hasNext())
            System.out.print(listIter.next());

        System.out.println();

        // 역방향 출력 → 54321
        while(listIter.hasPrevious())
            System.out.print(listIter.previous());
    }
}
```

(c)를 현재 위치라고 생각하고 설명해보겠다. <b>(1. 순방향, 2. 역방향)</b></p>

<p data-ke-size="size16"><b>1-1. <span style="color: #006dd7;">(c)</span> 1&nbsp; 2&nbsp; 3&nbsp; 4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는 <b>1</b>이고, previous는 없다.</p>
<p data-ke-size="size16">&rarr; next( ) / Output: 1</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>1-</b>2. 1 <span style="color: #006dd7;"><b>(c)</b></span> 2&nbsp; 3&nbsp; 4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>2</b>이고, previous는 <b>1</b>이다.</p>
<p data-ke-size="size16">&rarr; next( ) / Output: 1 2</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>1-</b>3. 1<span> </span><span>&nbsp;</span>2 <span style="color: #006dd7;"><b><b>(c)</b></b></span> 3&nbsp; 4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>3</b>이고, previous는<span>&nbsp;</span><b>2</b>이다.</p>
<p data-ke-size="size16">&rarr; next( ) / Output: 1 2 3</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>1-</b>4. 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span> </span><span>&nbsp;</span>3 <span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span> 4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>4</b>이고, previous는<span>&nbsp;</span><b>3</b>이다.</p>
<p data-ke-size="size16">&rarr; next( ) / Output: 1 2 3 4</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>1-</b>5. 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span><span>&nbsp;</span></span><span>&nbsp;</span>3<span> </span><span>&nbsp;</span>4 <b><span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span></b> 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>5</b>이고, previous는<span>&nbsp;</span><b>4</b>이다.</p>
<p data-ke-size="size16">&rarr; next( ) / Output: 1 2 3 4 5</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>1-</b>6. 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span><span>&nbsp;</span></span><span>&nbsp;</span>3<span><span>&nbsp;</span></span><span>&nbsp;</span>4<span> </span><span>&nbsp;</span>5 <b><b><span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span></b></b></b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span>없고, previous는<span> 5</span>이다.</p>
<p data-ke-size="size16">&rarr; next( ) 진행 불가 / while문 탈출</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b>2-1.<span>&nbsp;</span>1&nbsp; 2&nbsp; 3&nbsp; 4&nbsp; 5 <b><span style="color: #006dd7;">(c)</span></b></b></p>
<p data-ke-size="size16">&rarr; (c)의 next는 없고, previous는 5다.</p>
<p data-ke-size="size16">&rarr; previous( ) / Output: 5</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>2-</b>2. 1<span> </span><span>&nbsp;</span>2&nbsp; 3&nbsp; 4 <b><span style="color: #006dd7;"><b>(c)</b></span></b> 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>5</b>이고, previous는<span>&nbsp;</span><b>4</b>이다.</p>
<p data-ke-size="size16">&rarr; previous( )<span>&nbsp;</span>/ Output: 5 4</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>2-</b>3. 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span> </span><span>&nbsp;</span>3 <b><span style="color: #006dd7;"><b><b>(c)</b></b></span></b> 4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<b><span> 4</span></b>이고, previous는<span>&nbsp;</span><b>3</b>이다.</p>
<p data-ke-size="size16">&rarr; previous( )<span>&nbsp;</span>/ Output: 5 4 3</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>2-</b>4. 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span><span> <b><span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span></b></span></span><span>&nbsp;</span>3<span> </span><span>&nbsp;</span>4&nbsp; 5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>4</b>이고, previous는<span>&nbsp;</span><b>3</b>이다.</p>
<p data-ke-size="size16">&rarr; previous( )<span>&nbsp;</span>/ Output: 5 4 3 2</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>2-</b>5. 1<span><span> <b><b><span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span></b></b></span></span><span>&nbsp;</span>2<span><span>&nbsp;</span></span><span>&nbsp;</span>3<span><span>&nbsp;</span></span><span>&nbsp;</span>4<span> </span><span>&nbsp;</span>5</b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>2</b>이고, previous는<span>&nbsp;</span><b>1</b>이다.</p>
<p data-ke-size="size16">&rarr; previous( )<span>&nbsp;</span>/ Output: 5 4 3 2 1</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b><b>2-6</b>. <b><b><b><span style="color: #006dd7;"><b><b><b>(c)</b></b></b></span></b></b></b> 1<span><span>&nbsp;</span></span><span>&nbsp;</span>2<span><span>&nbsp;</span></span><span>&nbsp;</span>3<span><span>&nbsp;</span></span><span>&nbsp;</span>4<span><span>&nbsp;</span></span><span>&nbsp;</span>5<span>&nbsp;</span></b></p>
<p data-ke-size="size16">&rarr; (c)의 next는<span>&nbsp;</span><b>1</b>이고, previous는<span><span>&nbsp;없</span></span>다.</p>
<p data-ke-size="size16">&rarr; previous( ) 진행 불가 /<span>&nbsp;</span> while문 탈출</p>
