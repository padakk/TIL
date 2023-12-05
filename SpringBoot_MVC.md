# SpringBoot (23-12-05_TIL)(MVC & Layered Architecture)

<h3>1. MVC(Model View Controller)</h3>
디자인 패턴 중 하나로, 어플리케이션을 구성할 때 그 구성요소들을 세 가지의 역할로 구분한 패턴을 의미함.<br><br>
사용자 인터페이스로부터 비즈니스 로직을 분리하여 서로 영향 없이 쉽게 고칠 수 있는 설계 가능<br><br>

- Controller<br>
```
- Model 과 View 사이에서 브릿지 역할을 수행

- 앱의 사용자로부터 입력에 대한 응답으로 Model 및 View를 업데이트 하는 로직을 포함

- 사용자의 요청은 모두 Controller를 통해 진행되어야 한다.

- Controller로 들어온 요청은 어떻게 처리할 지 결정하여 Model로 요청을 전달한다.

Ex) 쇼핑몰에서 상품을 검색하면 해당 키워드를 Controller가 받아
    모델과 뷰에 적절하게 입력을 처리하여 전달한다.
```

- Model<br>
```
- 데이터를 처리하는 영역

- DB와 연동을 위한 DAO(Data Access Object)와 데이터의 구조를 표현하는 DO(Data Object)로 구성됨

Ex) 검색을 위한 키워드가 넘어오면 DB에서 관련된 상품의 데이터를 받아 View에 전달한다.
```

- View<br>
```
- 데이터를 보여주는 화면 자체의 영역

- 사용자 인터페이스(UI)요소들이 여기에 포함되며, 데이터를 각 요소에 배치함

- View에서는 별도의 데이터를 보관하지 않음

Ex) 검색 결과를 보여주기 위해 Model에서 결과 상품 리스트 데이터를 받음.
```

<br>

<h3>2. Layered Architecture</h3>

SpringBoot의 방식이며, 어플리케이션의 컴포넌트를 유사 관심사를 기준으로 레이어로 묶어 수평적으로 구성한 구조.<br>

어떻게 설계하느냐에 따라 용어와 계층의 수는 달라짐.(일반적으로 3 ~ 4계층 구조)<br>

각 Layer들은 다른 Layer들과 통신함.<br>

효율적인 개발과 유지보수를 위해 어플리케이션을 계층화하여 개발하는 것<br>

주로 중/대규모 어플리케이션에 사용함.<br>

- Presentation Layer<br>
```
- 어플리케이션의 최상단 계층

- 클라이언트의 요청을 해석하고 응답

- UI 또는 API 제공

- 비즈니스 로직 포함X Business Layer로 요청을 위임하여 반환받은 결과를 응답하는 역할만 수행

Ex) View, Controller가 이에 해당
```

- Business Layer<br>
```
- 어플리케이션이 제공하는 기능을 정의하고, 세부 작업을 수행

- Service Layer라고 칭하기도 함

Ex) Model, Service가 이에 해당
```

- Data Access Layer<br>
```
- 데이터베이스에 직접 접근하는 모든 작업을 수행

Ex) Model, Repository가 이에 해당
```
<br>
<h3>3. Layered Architecture의 예시</h3>

- Entity
```java
@Getter@Setter
@AllArgsConstructor
@Entity
public class MyUser {

    @Id
    private String userId;
    private String password;
}
```
- Controller
```java
@RestController
public class MyController {

    @Autowired
    private MyService myService;

    @GetMapping("/user/{userId}")//  /user/{userId}의 URI로 요청을 받는다.
    public ResponseEntity<?> userInfo(@PathVariable String userId){
                                 //  요청을 받으면 myService로 파라미터 userId만 전달
        MyUser user = myService.getUser(userId);
        return ResponseEntity.ok(user);
    }
}
```
- Service
```java
@Service
public class MyService{

    @Autowired
    private MyRepository myRepository;

    public MyUser getUser(String userId) {//  상위 계층(Controller)로부터 받은 userId 정보로 비즈니스 로직을 처리
        return myRepository.findById(userId);
    }
}
```
- Repository
```java
@Repository
public interface MyRepository extends JpaRepository<MyUser, String>{
        /*데이터베이스에 직접 접근하여 데이터베이스로부터 데이터를 받아와서, 
            상위 계층인 서비스계층으로 반환*/
}
```

<br>
<h3>4. 전반적인 동작흐름</h3>

```
클라이언트 요청 -> Controller -> Service -> Repository -> Service -> Controller -> 클라이언트 응답
```

상위 계층은 가장 가까운 하위 계층의 의존성을 주입받고 하위계층으로 요청을 전달,<br>

하위 계층은 요청을 처리하여 상위 계층으로 반환<br>
<br>
<h3>5. 계층설계의 장점</h3>

다른 계층의 역할을 침범하지 않는다.<br><br>
이에 따라 각 컴포넌트의 역할이 명확해지므로<br><br>
코드의 가독성과 기능 구현에 유리하고, 확장성이 좋아진다.<br><br>
각 계층이 독립적으로 작성되기 때문에 다른 레이어와의 의존성을 낮춰 단위테스트에 유리하다.