# 부재 : @Autowired 해도 null이 나오는 경우
## @Autowired로 오토와이어링에 실패해서 null이 나온다
- Bean의 이름이나, 타입, 클래스 상속관계, Annotation을 점검하고 아무리 맞춰봐도 계속 오토와이링이 되지않는다면 다음을 의심해볼 수 있다.
<br>

## Proxying Failure(프록시 객체 구성 실패)
### *Annotation은 본래 아무런 힘이 없다*
- Spring의 대부분 Bean은 여러 가공단계를 거쳐서 Framework가 사용할 수 있게 가공된다.
- Annotation 코드 자체적으론 아무런 기능이 없으며, 프로그램이 실행될 때 단지 해당 클래스/객체/메소드에 부가적인 정보가 정보가 실리는 기능 밖에 없다.
  ✔
