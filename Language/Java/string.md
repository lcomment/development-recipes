## **String 클래스**

&nbsp; Java를 접하기 전에 C를 접했다면 문자열을 다룰 때 '문자열 타입이 하나 있으면 되지, 왜 이렇게 귀찮게 사용해야 하지?' 라는 생각을 한번쯤은 해봤을 것이다. 문자열을 다루기 위해 char 배열이나 포인터를 사용해야 했다. 하지만 Java는 우리가 그토록 원했던 String Type을 제공, 정확히는 String Class를 미리 구현해 놓았다.

```java
public final class String implements java.io.Serializable, Comparable {
    private char[] value;
    // . . .
}
```

&nbsp; 한편, String 클래스는 저장된 문자열을 읽어올 수만 있고, 변경할 수는 없는 immutable 클래스다. 예를 들어 '+' 연사자를 이용해서 문자열을 결합하는 경우는 객체 내의 문자열을 바꾸는게 아니라 새로운 문자열이 담긴 String 객체를 생성하는 것이다. 따라서 문자열을 결합하는 것은 메모리 공간을 낭비하는 것과 같으므로 결합횟수를 최대한 줄이는 것이 좋다. 추가로 결합이나 추출 등 문자열을 다루는 작업이 많을 경우엔 String 클래스 대신 StringBuffer 클래스를 사용하는 것이 좋다.

```java
String s1 = "abc";
String s2 = "abc";
String s3 = new String("abc");
String s4 = new String("abc");

System.out.println(s1==s2);
System.out.println(s3==s4);
```

&nbsp; 위의 코드를 보자. 1, 2열은 문자열 리터럴 "abc"의 주소에 객체를 저장한 사례이고, 3, 4열은 새로운 String 객체를 생성한 사례다. 따라서 s1과 s2는 같은 문자열 "abc"의 주소를 가리키고 있고, s3와 s4는 같은 문자열이지만 서로 다른 주소를 가지고 있다. 위 코드를 실행해 출력 결과를 확인하면 완벽히 이해갈 것이다.

<br>

## **String 클래스의 메서드**

<table style="border-collapse: collapse; width: 100%; height: 867px;" border="1" data-ke-align="alignLeft" data-ke-style="style4">
<tbody>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px; text-align: center;"><span style="font-family: 'Noto Sans Demilight', 'Noto Sans KR';"><b>Method</b></span></td>
<td style="width: 20.1164%; height: 19px; text-align: center;"><span style="font-family: 'Noto Sans Demilight', 'Noto Sans KR';"><b>Explain</b></span></td>
<td style="width: 32.7908%; height: 19px; text-align: center;"><span style="font-family: 'Noto Sans Demilight', 'Noto Sans KR';"><b>Example code</b></span></td>
<td style="width: 15.465%; height: 19px; text-align: center;"><span style="font-family: 'Noto Sans Demilight', 'Noto Sans KR';"><b>Example Value</b></span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">char <span style="color: #ee2323;"><b>charAt</b></span>(int index)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 인덱스에 있는 문자 리턴</span></td>
<td style="width: 32.7908%; height: 19px;">&nbsp;</td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">c = 'e'</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int <span style="color: #ee2323;"><b>compareTo</b></span>(String str)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열과 사전순서로 비교하여 이전이면 음수, 이후면 양수 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int i = "aaa".compareTo("aaa");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int ii = "aaa".compareTo("bbb");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int iii = "bbb".compareTo("aaa");</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">i = 0</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">ii = -1</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">iii = 1</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <span style="color: #ee2323;"><b>concat</b></span>(String str)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열을 뒤에 덧붙여서 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "Hello";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s2 = s.concat(" !");</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">s2 = "Hello !"</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean <span style="color: #ee2323;"><b>contains</b></span>(CharSequence c)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 문자열이 포함돼 있는지 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "abcd";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean b = s.contains("bc");</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 22px;">
<td style="width: 31.6278%; height: 22px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean <b>endsWith</b>(Stringsuffix)</span></td>
<td style="width: 20.1164%; height: 22px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 문자열로 끝나는지 리턴</span></td>
<td style="width: 32.7908%; height: 22px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "abcd";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean b = s.endsWith("d");</span></td>
<td style="width: 15.465%; height: 22px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean <span style="color: #ee2323;"><b>equals</b></span>(Object obj)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열 비교 결과 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = new String("abc");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = new String("abc");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean b = s.equals(s1);</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean <b>equalsIgnoreCase</b>(String str)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열과 String 객체의 문자열을 대소문자 구분없이 비교하여 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = new String("abc");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = new String("ABC");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean b = s.<span style="background-color: #f9f9f9;">equalsIgnoreCase</span>(s1);</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int <span style="color: #ee2323;"><b>indexOf</b></span>(int ch)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">주어진 문자가 문자열에 존재하는지 확인하여 인덱스 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "Hello";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int idx1 = s.indexOf('o');</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int idx2 = s.indexOf('q');</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx1 =&nbsp; 4</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx2 = -1</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int<b> indexOf</b>(int ch, int pos)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">주어진 문자가 문자열에 존재하는지 정된 위치부터 확인하여 인덱스 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "Hello";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int idx1 = s.indexOf('e', 0);</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int idx2 = s.indexOf(<span style="background-color: #f9f9f9;">'e', 2)</span>;</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx1 =&nbsp; 1</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx2 = -1</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int<b> indexOf</b>(String str)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">주어진 문자열이 존재하는지 확인하여 인덱스 반환</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "ABCDE";</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int idx = s.indexOf("BC);</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx = 1</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>intern</b>( )</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열을 상수풀에 등록</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = new String("abc");</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s2 = new String("abc");<br />boolean b =&nbsp;<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (s1.intern( ) == s2.intern( );)</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int <b>lastIndexof</b>(char ch)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 인덱스 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;"><span style="background-color: #f9f9f9;">String s = "Hello";<br /></span>int idx = s.lastIndexOf('l');</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx = 3</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int <b>lastIndexOf</b>(String str)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 문자열을 객체의 문자열 끝에서 부터 찾아서 인덱스 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;"><span style="background-color: #f9f9f9;">String s = "Hello";<br /></span>int idx = s.lastIndexOf("ll");</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">idx = 2</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">int <span style="color: #ee2323;"><b>length</b></span>( )</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열의 길이 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;"><span style="background-color: #f9f9f9;">String s = "Hello";<br /></span>int len = s.length();</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">len = 5</span></td>
</tr>
<tr style="height: 19px;">
<td style="width: 31.6278%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <span style="color: #ee2323;"><b>replace</b></span>(char old,&nbsp; char new)</span></td>
<td style="width: 20.1164%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">old를 new로 바꾸어 리턴</span></td>
<td style="width: 32.7908%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;"><span style="background-color: #f9f9f9;">String s = "Hello";<br /></span>String rs = s.replace('e', 'a');</span></td>
<td style="width: 15.465%; height: 19px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">rs = "Hallo"</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>replace</b>(CharSequence old, CharSequence new)</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열 중 old를 new로 바꾸어 리턴</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;"><span style="background-color: #f9f9f9;">String s = "Hellollo";<br /></span><span style="background-color: #f9f9f9;">String rs = s.replace("ll", "LL");</span></span></td>
<td style="width: 15.465%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">re = "HeLLoLLo"</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>replaceAll</b>(String regex, String replacement)</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열 중 지정된 문자열과 일치하는 것을 새로운 문자열로 모두 변경하여 리턴</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String ab = "AABBAABB";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = ab.replaceAll("BB", "bb");</span></td>
<td style="width: 15.465%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">r = "AAbbAAbb"</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>replaceFirst</b>(String regex, String replacement)</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열 중 지정된 문자열과 일치한 것 중, 첫번째 것만 새로운 문자열로 변경하여 리턴</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String ab = "AABBAABB";</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = ab.replaceFirst("BB", "bb");</span></td>
<td style="width: 15.465%; height: 40px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">r = "AAbbAABB"</span></td>
</tr>
<tr style="height: 20px;">
<td style="width: 31.6278%; height: 20px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String[] <span style="color: #ee2323;"><b>split</b></span>(String regex)</span></td>
<td style="width: 20.1164%; height: 20px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열을 지정된 분리자로 나누어 문자열 배열에 담아 리턴</span></td>
<td style="width: 32.7908%; height: 20px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "010,1234,5678"</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String[] arr = s.split(",");</span></td>
<td style="width: 15.465%; height: 20px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">arr[0] = "010"</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">arr[1] = "1234"</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">arr[2] = "5678"</span></td>
</tr>
<tr style="height: 20px;">
<td style="width: 31.6278%; height: 20px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String[] <b>split</b>(String regex, int limit)</span></td>
<td style="width: 20.1164%; height: 20px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열을 지정된 분리자로 나누어 문자열 배열에 담아 리턴하는데, 문자열 전체를 지정된 수로 자름</span></td>
<td style="width: 32.7908%; height: 20px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "010,1234,5678"</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String[] arr = s.split(",", 2);</span></td>
<td style="width: 15.465%; height: 20px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">arr[0] = "010"</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">arr[1] = <br />&nbsp; &nbsp; &nbsp;"1234, 5678"</span></td>
</tr>
<tr style="height: 60px;">
<td style="width: 31.6278%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">static String<b> join</b>(String r, String[] s)</span></td>
<td style="width: 20.1164%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">여러 문자열 사이에 구분자를 넣어서 결합 후 리턴</span></td>
<td style="width: 32.7908%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "010,1234,5678"</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String[] arr = s.split(",");</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String str = String.join("-", arr)</span></td>
<td style="width: 15.465%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">str = </span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">&nbsp; 010-1234-5678</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean <b>startsWith</b>(String prefix)</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">주어진 문자열로 시작하는지 리턴</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "abcd";</span><br /><span style="background-color: #f9f9f9; font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">boolean b = s.endsWith("a");</span></td>
<td style="width: 15.465%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">b = true</span></td>
</tr>
<tr style="height: 60px;">
<td style="width: 31.6278%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <span style="color: #ee2323;"><b>substring</b></span>(int begin)</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <span style="color: #ee2323;"><b>substring</b></span>(int begin, int end)</span></td>
<td style="width: 20.1164%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">시작 위치부터 끝 위치 범위에 포함된 문자열 리턴</span></td>
<td style="width: 32.7908%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "I like Java";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String c = s.substring(3, 6);</span></td>
<td style="width: 15.465%; height: 60px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">c = "ike "</span></td>
</tr>
<tr style="height: 80px;">
<td style="width: 31.6278%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>toLowerCase</b>( )</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <b>toUpperCase</b>( )</span></td>
<td style="width: 20.1164%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String 객체에 저장된 문자열을 소문자(또는 대문자)로 변환하여 반환</span></td>
<td style="width: 32.7908%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "abCdE";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String l = s.toLowerCase();</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String u = s.toUpperCase();</span></td>
<td style="width: 15.465%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">l = "abcde"</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">u = "ABCDE"</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String <span style="color: #ee2323;"><b>toString</b></span>( )</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String 객체에 저장돼 있는 문자열 반환</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "Hello";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s1 = s.toString();</span></td>
<td style="width: 15.465%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">s1 = "Hello"</span></td>
</tr>
<tr style="height: 40px;">
<td style="width: 31.6278%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String<b> trim</b>( )</span></td>
<td style="width: 20.1164%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">문자열 양쪽 끝에 있는 공백을 제거하여 리턴</span></td>
<td style="width: 32.7908%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = "&nbsp; &nbsp;Hi&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;";</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String rs = s.trim();</span></td>
<td style="width: 15.465%; height: 40px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">rs = "Hi"</span></td>
</tr>
<tr style="height: 80px;">
<td style="width: 31.6278%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">static String <span style="color: #ee2323;"><b>valueOf</b></span>(type t)</span></td>
<td style="width: 20.1164%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">지정된 값을 문자열로 변환하여 반환</span></td>
<td style="width: 32.7908%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s = String.valueOf(true);</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">java.util.Date dd = new java.util.Date();</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">String s2 = String.valueOf(dd);</span></td>
<td style="width: 15.465%; height: 80px;"><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">s1 = "true"</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">s2 = </span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">"Mon Jul 19 01:</span><br /><span style="font-family: AppleSDGothicNeo-Regular, 'Malgun Gothic', '맑은 고딕', dotum, 돋움, sans-serif;">50:30 KST 2021"</span></td>
</tr>
</tbody>
</table>
