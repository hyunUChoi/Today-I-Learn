# 부재 : @Autowired 해도 null이 나오는 경우
## @Autowired로 오토와이어링에 실패해서 null이 나온다
- Bean의 이름이나, 타입, 클래스 상속관계, Annotation을 점검하고 아무리 맞춰봐도 계속 오토와이링이 되지않는다면 다음을 의심해볼 수 있다.
<br>

## Proxying Failure(프록시 객체 구성 실패)
### *Annotation은 본래 아무런 힘이 없다*
- Spring의 대부분 Bean은 여러 가공단계를 거쳐서 Framework가 사용할 수 있게 가공된다.
- Annotation 코드 자체적으론 아무런 기능이 없으며, 프로그램이 실행될 때 단지 해당 클래스/객체/메소드에 부가적인 정보가 정보가 실리는 기능 밖에 없다.<br>
✔ 즉, 일반 자바 어플리케이션(public static void main을 통해서 실행되는 애들)은 @Autowired 어노테이션이 있어도 아무런 처리도 하지 못하고 넘어간다!
<br>

### *Spring Framework는 Annotation이 의미있게 돌아가도록 작업을 수행한다*
- 그러면 Spring은 무슨 처리를 하길래 @Aurowired가 Spring이 의도한대로 동작하는지에 대한 의문점이 생긴다.
- ~~이 세상에 공짜는 없다.~~ Spring은 Bean 하나를 사용할 수 있게 만들기 위해서 다음과 같은 작업을 수행한다.
- 중간에 생략된 절차가 많지만 설명하는데 큰 지장은 없기 때문에 우선은 생략한다.(~~더 이상의 자세한 설명은 생략한다.~~)
---
1. Framework가 ConsoleApplication 혹은 WebApplication 가동 절차에 따라서 가동한다.
2. 진입점(Entry Point)로 지정한 설정 파일을 인식한다.
   - XML 파일인 경우 파일을 직접 경로에서 열게되고, JavaConfig라면 classPath 내 지정된 클래스 파일을 설정파일로 읽는다.
4. 아무것도 준비되어있지 않은 Bean 객체를 생성한다.
   - XML 파일인 경우 Constructor-args로 전달받은 객체를 받아 객체를 생성하고, JavaConfig라면 명시적으로 java 코드를 통해 생성자 매개변수를 넣어 객체를 생성한 후 return한다.
---
- 아직까지 각 Bean 내부의 Annotaion은 전혀 처리되지 않았다. 안에 들려있는 @Autowired, @Resource, @Value는 위에서 말했듯이 무언가를 실행하는 코드로서는 아무런 기능이 없다.
- 이 상태에서 더 이상 진행되지 않으면 @Autowired로 표시한 멤버 변수들은 **null**이다.
- 모든 Annotation이 처리된 상태로 실행되려면 이에 필요한 소정의 코드를 실행시간(Runtime)에 생성하여 집어넣어야한다.
- 
