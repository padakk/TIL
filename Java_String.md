# Java (23-11-28_TIL)(String)

<h3>1. StringBuilder, StringBuffer</h3>
String 클래스와 달리 가변(mutable) 클래스에 해당한다.<br><br>

- 불변(immutable) 클래스<br>
최초로 객체를 생성한 뒤 상태를 변경할 수 없는 클래스.<br> 
String, Integer, Float, Double, Long, Character 등 참조형, Wrapper class 가 이에 해당한다.<br>

- 가변(mutable) 클래스<br>
반대로 객체를 생성한 뒤 상태를 변경할 수 있는 클래스.<br>
기존 객체의 상태가 변경되어 구성된다.<br>
StringBuffer, StringBuilder, ArrayList, HashMap, HashSet, TreeMap, TreeSet 등이 있다.


<h3>2. 사용 이유?</h3>

- 많은 문자열을 연결시키거나, 변경해야 할 때 불변클래스를 사용하면 메모리가 불필요하게 많이 사용된다.<br>


- 사용예제(StringBuilder, StringBuffer)
```java
public class Main
{
    public static void main(String[] args)
    {
        StringBuilder sb = new StringBuilder();
        sb.append("문자열 ").append("연결"); //문자열 추가
//      String str = stringBuilder;         // 에러
        String str = sb.toString();         // String으로 변환
        str = String.valueOf(sb);           // String으로 변환
        
        System.out.println(sb);
        System.out.println(str);
        // 두 println()은 동일한 결과

        sb.setCharAt(3, 'A');   //특정 문자변경(변경할 인덱스, 변경할 문자)
        sb.insert(2, "ccc")/    //해당 인덱스 위치에 문자열 추가
        sb.replace(2, 4, "to"); //해당 위치 문자열 교체

        sb.setLength(3);        //문자열 길이 바꾸기

    }
}
```

<h3>3. StringBuilder와 StringBuffer의 차이점</h3>

- StringBuilder는 싱글쓰레드 환경에서 더 좋은 성능을 발휘한다.<br><br>
- StringBuffer는 멀티쓰레드 환경에서 더 좋은 성능을 발휘한다.<br><br>
- 통상적으로 StringBuffer를 더 많이 사용하지만<br>
StringBuffer는 클래스 안에 lock기능이 존재해서 deadlock의 위험성이 있다.<br><br>