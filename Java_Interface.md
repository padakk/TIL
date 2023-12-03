# Java (23-11-27_TIL)

+ Interface란?<br>
&nbsp;&nbsp;추상메서드의 집합<br>
모든 멤버가 public abstract(생략 가능)<br><br>

+ 추상클래스와 Interface의 차이<br>
&nbsp;&nbsp;추상 클래스는 추상메서드가 포함된 클래스이다.<br>
즉, 인스턴스 멤버와 같은 것들이 포함이 가능하다.<br><br>
&nbsp;&nbsp;반면에 Interface는 추상메서드만 포함이 가능하다.<br>
(JDK 1.8부터 편의성을 위해 default class, static class는 허용<br>그러나, 이로 인해 충돌문제가 생길 수 있다. 이는 Overriding으로 해결한다.)

+ Interface에 대한 설명<br>
&nbsp;&nbsp;Interface는 Object Class를 조상으로 삼지 않는다.<br>
(Interface만 조상으로 삼는다.)<br><br>
&nbsp;&nbsp;Class와 다르게 다중상속이 가능하다.<br>
(추상메서드의 집합이라 충돌문제를 고려할 필요가 없기 때문이다.)<br><br>
&nbsp;&nbsp;Interface는 반환타입으로 지정이 가능하다.<br>
(해당 인터페이스에서 구현된 객체를 반환한다.)<br><br>

+ Interface를 사용하는 이유(장점)<br>
&nbsp;&nbsp;1. 대상 간의 연결, 대화, 소통을 돕는 <u>중간다리</u> 역할을 한다.<br><br>
&nbsp;&nbsp;2. 선언과 구현을 분리시킬 수 있도록 해준다.<br>
&nbsp;&nbsp;(알맹이와 껍데기를 분리해준다.)<br><br>
&nbsp;&nbsp;3. 개발 시간을 단축할 수 있다.<br><br>
&nbsp;&nbsp;4. 변경에 유리한 설계가 가능하다.<br><br>
&nbsp;&nbsp;5. 표준화가 가능하다.<br><br>
&nbsp;&nbsp;6. 관계없는 클래스들끼리 관계를 맺어줄 수 있다.<br>
&nbsp;&nbsp;(단일상속의 불편한 점을 해결)<br><br>

+ 예제<br>
```java
abstract class Unit2 {
	int x, y;
	abstract void move(int x, int y);
	void stop() { System.out.println("멈춥니다. ");}
}

interface Fightable {			//인터페이스의 모든 메서드는 public abstract (예외없이)
	void move(int x, int y);	//public abstract가 생략됨
	void attack(Fightable f);	//public abstract가 생략됨
}

class Fighter extends Unit2 implements Fightable {
	public void move(int x, int y) {	//오버라이딩 규칙 : 조상(public)보다 접근제어자가 좁으면 안된다.
		System.out.println("[" + x + "," + y + "]로 이동");
	}
	public void attack(Fightable f) {
		System.out.println(f + "를 공격");
	}
	
	Fightable getFightable() {
		Fighter f = new Fighter();
		return (Fightable)f;
	}
}

public class FighterTset {
	public static void main(String[] args) {
		
		Fighter f2 = new Fighter();
		Fightable f3 = f2.getFightable();	//형변환 해주기(타입 일치시키기)
		
//		Fighter f = new Fighter();
		Fightable f = new Fighter();
		Unit2 u = new Fighter();
		
		u.move(100, 200);
//		u.attack(new Fighter());	//Unit2에는 attack()가 없어서 호출불가
		u.stop();
		
		
		f.move(100, 200);
		f.attack(new Fighter());
//		f.stop();				//Fightable에는 attack()가 없어서 호출불가
	}							//인터페이스에도 다형성이 적용된다.
}
```
```java
class A {
//	public void method(B b)		// 의존적이다. 변경사항이 생기면 둘 다 바꿔줘야 함
	public void method(I i) {	// 인터페이스 I를 구현한 놈들만 들어와라
		i.method();
	}
}

//B클래스의 선언과 구현을 분리
interface I {
	public void method();
}

class B implements I {
	public void method() {
		System.out.println("B클래스의 메서드");
	}
}

class C implements I {
	public void method() {
		System.out.println("C클래스의 메서드");
	}
}

public class InterfaceTest {
	public static void main(String[] args) {
		A a = new A();
		a.method(new B());	
	}
}
```
