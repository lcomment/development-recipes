## **StringBuffer 클래스**

&nbsp; String 클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없다. replace( )와 같은 메서드는 문자열을 새로 생성하는 것이지 변경하는 메서드가 아니다. 따라서 오버헤드가 존재하게 된다. 오늘 알아볼 StringBuffer 클래스는 변경이 가능하다. 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며, StringBuffer 인스턴스를 생성할 때 크기를 지정할 수 있다. 따라서 우리는 편집할 문자열의 길이를 고려하여 충분한 길이를 설정해주는 것이 좋다. 편집 중 길이를 늘려주는 작업을 수행한다면 작업효율이 떨어지기 때문이다.

```java
public final class StringBuffer implements java.io.Serializable {
	private char[] value;
    . . .
}
```

&nbsp; 위의 코드는 StringBuffer이다. 어디서 많이 본듯한 인스턴스 변수가 선언돼 있다. 바로 String 클래스다. StringBuffer 인스턴스(객체)가 생성될 때, char형 배열이 생성되며 이 때 생성된 char형 배열을 인스턴스변수 value가 참조하게 된다.

<br>

## **StringBuffer 생성자**

&nbsp; StringBuffer 인스턴스를 생성할 때는 생성자 StringBuffer(int length)를 사용하는데, 이 때 버퍼의 크기를 지정해주지 않으면 디폴트 값(default value)은 16, 즉 16개의 문자를 저장할 수 있는 크기의 버퍼가 생성된다.

```java
public StringBuffer(int length) {
	value = new char[length];
    shared = false;
}

public StringBuffer(){
	this(16);
}
public StringBuffer(){
	this(str.length() + 16);
    append(str);
}
```

&nbsp; 위의 코드는 StringBuffer 클래스의 일부다. 버퍼의 크기를 생성자가 오버로딩을 통해 여러 메서드로 구분돼 있다.

<br>

## **StringBuffer 변경**

&nbsp; String 클래스와 달리 StringBuffer는 내용을 변경할 수 있다고 위에서 설명했다. 아래의 코드를 한 번 보자.

```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123");	// sb = abc123

StringBuffer sb2 = sb.append("zz");
StringBuffer sb3 = sb.append("khs").append("exk");

System.out.println(sb); // abc123zzkhsexk
System.out.println(sb2); // abc123zzkhsexk
System.out.println(sb3); // abc123zzkhsexk
```

&nbsp; 위의 코드를 통해 우리는 두 가지만 기억하면 된다. 먼저 StringBuffer는 참조 타입(reference type)이기 때문에 대입 연산자를 통해 sb2에 sb를 대입시키면 같은 주소를 가리키게 된다. 따라서 sb나 sb1, 이후에는 sb3까지 어느 하나만 값을 변경해도 셋 다 영향을 받는다.

&nbsp; 두 번째는 append( ) 메서드에 있다. sb3를 생성할 때 우리는 의문점이 든다. append( ) 메서드가 두 번 쓰였기 때문인데, append( )는 리턴타입이 StringBuffer, 즉 자신의 주소를 리턴하기 때문에 위와 같이 쓸 수 있는 것이다.

<br>

## **StringBuffer 비교**

&nbsp; `비교`라는 타이틀을 보고 우리는 `==` 연산자와 `equals( )` 메서드를 떠올리는데, String 클래스의 equals( ) 메서드와 StringBuffer 클래스의 equals( ) 메서드는 차이가 있다. String 클래스에서는 equals( ) 메서드를 오버라이딩 하여 문자열의 내용을 비교하도록 구현돼 있지만, StringBuffer는 오버라이딩하지 않아서 `equals( )` 메서드나 `==` 연산자의 결과가 같다.

&nbsp; 그럼 어떻게 비교할까? 다행히도 toString( ) 메서드는 오버라이딩 되어 있다. 밑의 코드를 보면서 StringBuffer의 비교법에 대해 이해해보자.

```java
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

System.out.println(sb==sb2); // false
System.out.println(sb.equals(sb2)); // false

String s = sb.toString();
String s2 = sb2.toString();

System.out.println(s.equals(s2)); // true
```

## **👀 StringBuilder와 StringBuffer**

&nbsp; StringBuffer는 멀티쓰레드에 안전하도록 동기화돼 있다. 동기화는 StringBuffer의 성능을 저하시킨다. 따라서 멀티 쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어지게 된다.

&nbsp; StringBuilder는 StringBuffer에서 쓰레드의 동기화만 뺀 것이다. 완전히 똑같은 기능으로 적성돼 있기에 코드에서도 StringBuffer를 StringBuilder로 바꾸기만 하면 된다. StringBuffer도 충분히 성능이 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하고는 StringBuilder를 사용할 필요는 없지만, 코딩테스트에서 특별한 경우가 없다면 StringBuilder를 사용하도록 하자.

</br>

## **StringBuffer 생성자와 메서드**

<table style="border-collapse: collapse; width: 100%; height: 365px;" border="1" data-ke-align="alignLeft" data-ke-style="style12">
<tbody>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;"><b>Method</b></td>
<td style="width: 28.9148%; height: 20px;"><b>Explanation</b></td>
<td style="width: 36.5891%; height: 20px;"><b>Example</b></td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer( )</td>
<td style="width: 28.9148%; height: 20px;">16문자를 담을 수 있는 버퍼를 <br />가진 StringBuffer 객체 생성</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer();</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer(int length)</td>
<td style="width: 28.9148%; height: 20px;">지정된 개수의 문자를 담을 수 <br />있는 버퍼를 가진 StringBuffer <br />객체 생성</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer(10);</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer(String str)</td>
<td style="width: 28.9148%; height: 20px;">지정된 문자열 값을 갖는 <br />StringBuffer 객체 생성</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer("abc");</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer append(boolean b)<br />StringBuffer append(char c)<br />StringBuffer append(char[] str)<br /><span style="background-color: #efefef;">StringBuffer append(double d)<br /><span style="background-color: #efefef;">StringBuffer append(float f)<br /></span></span><span style="background-color: #efefef;">StringBuffer append(int i)<br /><span style="background-color: #efefef;">StringBuffer append(long l)<br /><span style="background-color: #efefef;">StringBuffer append(Object obj)<br /><span style="background-color: #efefef;">StringBuffer append(String str)</span></span></span></span></td>
<td style="width: 28.9148%; height: 20px;">매개변수로 입력된 값을 문자열로<br />반환하여 StringBuffer 객체가 저장<br />하고 있는 문자열의 뒤에 덧붙임</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer("abc");<br />StringBuffer sb2 = sb.append(true);<br /><br />sb.append('d');<br />sb.append(10.0f);<br /><br />// sb = "abctrue10.0"<br />// sb2 = "abctrue10.0"</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">int capacity( )</td>
<td style="width: 28.9148%; height: 20px;">버퍼 크기 반환</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new SringBuffer(20);<br />int bf = sb.capacity();&nbsp; // bf = 20</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">char charAt(int index)</td>
<td style="width: 28.9148%; height: 20px;">지정된 인덱스의 문자 반환</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new SringBuffer("abc");<br />char c = sb.charAt(2);&nbsp; // c = 'c'</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer delete(int start, int end)</td>
<td style="width: 28.9148%; height: 20px;">시작 위치부터 끝 위치 사이의 <br />문자 제거</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb= new StringBuffer("12345");<br />sb.delete(1, 3);&nbsp; &nbsp; &nbsp; // sb = "145"</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer deleteCharAt(int index)</td>
<td style="width: 28.9148%; height: 20px;">지정된 인덱스의 문자 제거</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb= new StringBuffer("12345");<br />sb.delete(2);&nbsp; &nbsp; &nbsp; // sb = "1245"</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer insert(int pos, boolean b)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, char c)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, char[] c)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, double d)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, float f)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, int i)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, long l)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, Object obj)<br /><span style="background-color: #efefef;">StringBuffer insert(int pos, String str)</span></span></span></span></span></span></span></span></td>
<td style="width: 28.9148%; height: 20px;">두 번째 매개변수로 받은 값을 문자열로 변환하여 지정된 위치에<br />삽입</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer("1234");<br />sb.append(2, '.');<br /><br />// sb = "12.34"</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">int length( )</td>
<td style="width: 28.9148%; height: 20px;">문자열 길이 반환</td>
<td style="width: 36.5891%; height: 20px;">StringBuffer sb = new StringBuffer("1234");<br />int len = sb.length();&nbsp; &nbsp;// len = 4</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">StringBuffer replace(int start, int end, <br />String str)</td>
<td style="width: 28.9148%; height: 20px;">시작 위치부터 끝 위치 사이의<br />문자들을 주어진 문자열로 변경</td>
<td style="width: 36.5891%; height: 20px;"><span style="background-color: #f9f9f9;">StringBuffer sb = new StringBuffer("1234");sb.replace(1, 2, "AB");<br /></span><br />// sb = "1AB34"</td>
</tr>
<tr style="height: 25px;">
<td style="width: 34.496%; height: 25px;">StringBuffer reverse( )</td>
<td style="width: 28.9148%; height: 25px;">StringBuffer 객체에 저장돼 있는<br />문자열의 순서를 거꾸로 나열</td>
<td style="width: 36.5891%; height: 25px;"><span style="background-color: #f9f9f9;">StringBuffer sb = new StringBuffer("1234");sb.reverse();<br /><br />// sb = "4321"</span></td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">void setCharAt(int index, char ch)</td>
<td style="width: 28.9148%; height: 20px;">지정된 인덱스의 문자를 주어진 <br />문자로 변경</td>
<td style="width: 36.5891%; height: 20px;"><span style="background-color: #f9f9f9;">StringBuffer sb = new StringBuffer("abc");<br />sb.setCharAt(1, 'ㄴ');<br /></span><br />// sb = "aㄴc"</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">void setLength(int newLength)</td>
<td style="width: 28.9148%; height: 20px;">지정된 길이로 문자열의 길이 변경<br />(나머지 공간은 공백으로 채움)</td>
<td style="width: 36.5891%; height: 20px;"><span style="background-color: #f9f9f9;">StringBuffer sb = new StringBuffer("abc");<br /></span>sb.setLength(5);<br /><br />// sb = "abc&nbsp; "</td>
</tr>
<tr style="height: 20px;">
<td style="width: 34.496%; height: 20px;">String toString( )</td>
<td style="width: 28.9148%; height: 20px;">StringBuffer 객체의 문자열을 <br />String으로 반환</td>
<td style="width: 36.5891%; height: 20px;"><span style="background-color: #f9f9f9;">StringBuffer sb = new StringBuffer("abc");<br /></span>String s = sb.toString();<br /><br />// s ="abc"</td>
</tr>
<tr style="height: 40px;">
<td style="width: 34.496%; height: 40px;">String substring(int start)<br />String substring(int start, int end)</td>
<td style="width: 28.9148%; height: 40px;">시작 위치부터 끝 위치 사이의<br />문자열 반환<br /><br />(만약 시작위치만 지정하면 시작 위치부터 문자열 끝까지 뽑아서 반환)</td>
<td style="width: 36.5891%; height: 40px;">StringBuffer sb = new StringBuffer("abcd");<br /><br />String s = sb.substring(1, 3);<br />String s2 = sb.substring(1);<br /><br />// s = "bc"<br />// s2 = "bcd"</td>
</tr>
</tbody>
</table>
