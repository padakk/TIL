# Java (23-12-07_TIL)(Compare)

<h3>1. Comparable, Comparator</h3>
<u>객체 비교를 위한</u> 인터페이스에 해당한다.<br>
이를 사용하기 위해서는 인터페이스 내의 메서드를 <u>반드시 구현</u>해야 한다.<br>
구현해야 할 추상 메서드는 각 한 개씩이다. (나머지는 default, static 메서드에 해당)<br><br>


<h3>2. Comparable</h3>

compareTo(T o) 를 구현해주어야 함 (Override)<br>
자기 자신과 매개변수 객체를 비교<br>
lang package(import 필요 x)<br><br>


- 사용예제
```java
class Student implements Comparable<Student> {
 
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
	@Override
	public int compareTo(Student o) {
    
		// 자기자신의 age가 o의 age보다 크다면 양수
		if(this.age > o.age) {
			return 1;
		}
		// 자기 자신의 age와 o의 age가 같다면 0
		else if(this.age == o.age) {
			return 0;
		}
		// 자기 자신의 age가 o의 age보다 작다면 음수
		else {
			return -1;
		}
	}
}
```
- 두 비교대상의 값 차이를 반환(결과는 동일)
```java
    @Override
    public int compareTo(Student o) {
    
            /*
            * 만약 자신의 age가 o의 age보다 크다면 양수가 반환 될 것이고,
            * 같다면 0을, 작다면 음수를 반환할 것이다.
            */
            return this.age - o.age;
    }
```
<br>
<h3>3. Comparator</h3>

compare(T o1, T o2)를 구현해주어야 함 (Override)<br>
두 매개변수의 객체를 비교<br>
util package(import 필요)<br><br>


- 사용 예제
```java
import java.util.Comparator;	// import 필요
class Student implements Comparator<Student> {
 
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
	@Override
	public int compare(Student o1, Student o2) {
    
		// o1의 학급이 o2의 학급보다 크다면 양수
		if(o1.classNumber > o2.classNumber) {
			return 1;
		}
		// o1의 학급이 o2의 학급과 같다면 0
		else if(o1.classNumber == o2.classNumber) {
			return 0;
		}
		// o1의 학급이 o2의 학급보다 작다면 음수
		else {
			return -1;
		}
	}
}
```
- 두 비교대상의 값 차이를 반환(결과는 동일)
```java
	@Override
	public int compare(Student o1, Student o2) {
 
		/*
		 * 만약 o1의 classNumber가 o2의 classNumber보다 크다면 양수가 반환 될 것이고,
		 * 같다면 0을, 작다면 음수를 반환할 것이다.
		 */
		return o1.classNumber - o2.classNumber;
	}
```
- Comparator의 활용(익명 클래스(익명 객체))
```java
import java.util.Comparator;
 
public class Test {
	public static void main(String[] args)  {
 
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		Student c = new Student(15, 3);	// 15살 3반
			
		// comp 익명객체를 통해 b와 c객체를 비교한다.
		int isBig = comp.compare(b, c);
		
		if(isBig > 0) {
			System.out.println("b객체가 c객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("b객체가 c객체보다 작습니다.");
		}
		
	}
	
	public static Comparator<Student> comp = new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			return o1.classNumber - o2.classNumber;
		}
	};
}
 
class Student {
 
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
}
```
<br>
<h3>4. 주의사항</h3>

두 비교대상 간의 값 차이를 return하게 되면
<u>Overflow</u>가 발생할 수 있기 때문에 이를 염두해야한다.