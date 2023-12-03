# Java (23-11-28_TIL)(Array and List)

<h3>배열을 List로 변환</h3>

1. 반복문
```java
Integer[] arr = {1, 2, 3};             //Wrapper class
List<Integer> list = new ArrayList<>();//Primitive type은 다른방법을 활용

for(Integer num : arr) {
    list.add(num);                     //반복문을 활용하여 하나씩 추가
}
```
2. Collections class(addAll() 메서드)
```java
Integer[] arr = {1, 2, 3};             //Wrapper class
List<Integer> list = new ArrayList<>();//Primitive type은 다른방법을 활용
Collections.addAll(list, arr);         //addAll() 메서드 호출
```
3. Stream API (JDK 1.8이상)
```java
Integer[] arr = {1, 2, 3};             //Wrapper class
                                       //Collectors.toList()
List<Integer> list = Arrays.stream(arr).collect(Collectors.toList());
```
4. Primitive Type 배열을 변환(boxed() 메서드)
```java
int[] arr = {1, 2, 3};                 //Primitive Type
List<Integer> list = Arrays.stream(arr)
                           .boxed()    //Primitive -> Wrapper
                           .collect(Collectors.toList());
                                       //Collectors.toList()
```

<h3>List를 배열로 변환</h3>

1. List.toArray()
```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);

Integer[] arr = new Integer[list.size()];         // Type 일치
list.toArray(arr);

//Integer[] arr = list.toArray(new Integer[0]);   >> 위 두 문장과 동일
```
2. Stream API
```java
Integer[] arr = list.stream().toArray(Integer[]::new);
```
3. Primitive type 배열로 변환
```java
//첫 번째 방법 : 반복문
int[] arr = new int[list.size()];      // size()
for(int i=0; i<list.size(); i++){      // 반복문 사용
    arr[i] = list.get(i);              // get()
}



//두 번째 방법 : Stream API
int[] arr = list.stream().mapToInt(i->i).toArray();
                                       // mapToInt()
                                       // mapToDouble()
                                       // mapToLong() 등등
```
<h3>List 요소 다중삭제</h3>

```java
list.subList(0, 4).clear();            // subList().clear()
            //0번째 ~ 3번째 인덱스 삭제 (마지막 포함안됨)

list.removeIf(str -> str.startsWith("f"));
list.removeIf(str -> str.contains("c"));//removeIf()
```