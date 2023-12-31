# Java (23-11-28_TIL)(Iterator)

<h3>1. Iterator 란?</h3>
자바의 컬렉션(Collection)에 저장되어 있는 요소들을 순회하는 인터페이스<br><br>

- Collection?<br>
자바에서 제공하는 자료구조들의 인터페이스로 List, ArrayList, Stack, Quque, LinkedList 등이 이를 상속받고있다.<br> 
즉, 이러한 컬렉션 인터페이스를 상속받는 클래스들에 대해 Iterator 인터페이스 사용이 가능하다.


<h3>2. Iterator 사용 이유?</h3>

- 컬렉션 프레임워크에 대해 공통으로 사용이 가능<br> 
- 사용법이 비교적 간단함


```java
public class IteratorTest {
    public static void main(String[] args) {
        
        List<Integer> list = new ArrayList<Integer>(); //ArrayList 생성
 
        for(int i = 0;i <= 100; i++) {
            list.add(i);
        }
        
        Iterator<Integer> iterator = list.iterator();  //list에 대한 Iterator 생성
        
        while(iterator.hasNext()) {     //반복문 내부에서 요소의 추가, 제거 등에서 오는 에러 방지 가능
            int data = iterator.next(); //다음 요소 반환
            System.out.print(data);
            iterator.remove();          //요소 제거
        }   
    }  
}
```

<h3>3. Iterator와 반복문</h3>

```java
 public void linkedListTest() {
        LinkedList<Integer> list = new LinkedList<Integer>();
        
        for(int i = 0;i <= 100; i++) {
            list.add(i);    //요소 추가
        }
        
        for(int i = 0; i<= 100; i++) {
            list.get(i);    //get(i) 메서드는 시작 주소부터 i인덱스 까지 요소를 밟아가며 조회한다.
        }                   //i번째 값인 경우 내부적으로 i번이 반복된다. 
                            //0번째 인덱스부터 100번째 인덱스까지의 반복횟수는 1, 2, 3, ..., 101번 이므로 총 5151번


        Iterator<Integer> itr = list.iterator();
        while(itr.hasNext()) { //순차적으로 조회(총 반복횟수가 101번)
            int num = itr.next();   
            System.out.print(num);
        }
    }                       
```

상대적으로 Iterator의 반복횟수가 훨씬 적지만, Iterator의 객체 생성 부분에서 시간소요가 굉장히 크다.<br>
오히려 속도 면에서 Iterator가 조금 더 느리다. (차이가 크지는 않음)