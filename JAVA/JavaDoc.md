# 실전 JavaDoc 작성을 위한 Tip
- 개발하면서 내가 개발한 부분이 아닌 부분에서 수정이 필요해서 보는데 무슨말인지 하나도 모르겠는 상황이 종종 있다.
- 그리고 내가 개발한 부분인데도 시간이 지나서 정확한 기능 정의가 안될 때도 있다. ~~청년 치매가 이걸~~
- 이럴때를 대비해서 JavaDoc을 작성하는 방법을 정리하자
  
## 문서화의 중요성
- ~~일단 간지난다.~~ 사람의 기억력은 무한하지 않다.
- 메소드 이름이나 클래스 이름만으로 파악할 수 있는 기능은 한계가 있다.
- 어떤 이유로든 **'상식선에서'** 어긋난 코드가 작성될 상황이 생긴다면, 미래의 나와 후임을 위한 이력을 남겨 두어야한다.

## JavaDoc 
- /** 로 시작해서 */ 로 종료하는 일반적 Java 주석에 문서화 표준
- 일반적으로 클래스, 메소드, 멤버변수, 상수에 작성

## JavaDoc 구조
```
/**
 * javaDoc 사용법을 설명한다.
 *
 * <pre>
 *   javaDoc을 작성하는 방법은 이러쿵 저러쿵 밑에 설명문에 적혀있다.
 * </pre>
 *
 * @author
 * @param 
 * @thorws
 * @return
 */
public String javaDoc(final Object object) {
  ~~~
}
```
1. 첫 줄은 반드시 **'마침표'** 로 끝나는 명료한 문장으로 메소드의 기능을 서술한다. 너무 단순하게 '요청 가공값 리턴'과 같은 식으로 작성해선 안되고, 또한 너무 자세하게 적어도 안된다.
2. JavaDoc의 기본 문서는 HTML과 호환되는 문법구조를 가지고 있다. 따라서, < 와 > 같은 문자를 직접적으로 표현하려면 <pre> 태그로 감싼 부분에 표현한다. 또한 b, i, font, ol, ul 등 다양한 태그를 지원한다,
3. @태그는 파라미터, 리턴값, 예외 처리 등을 설명한다.

## @태그
### 🚨주의사항 - 관례상 @param, @return, @throws 태그의 설명에는 마침표를 붙이지 않는다.
- @param : 메소드의 파라미터가 무엇이고 어떤 값이 들어와야하는지 설명한다. 타입은 메소드 작성 시 이미 선언되어있기 때문에 상세하게 언급할지 말지는 선택사항이다.
  - 문법 구조 : @param (변수명) (설명)<br><br>
- @return : 메소드의 리턴값이 상황에 따라 어떻게 나오는지 설명한다.
  - 문법 구조 : @return (설명)<br><br>
- @thorws : 해당 메소드가 발생시킬 수 있는 예외에 대해 설명한다.
  - 문법 구조 : @thorws (예외 클래스명) (설명)<br><br>
- @deprecated : 해당 메소드 사용이 권장되지 않는 메소드임을 설명할 때 사용한다.
  - 문법 구조 : @deprecated (설명)<br><br>
- @exception : 발생할 수 있는 Exception을 설명한다.
  - 문법 구조 : @exception (설명)<br><br>
- @see : @deprecated와 주로 쌍을 이루는 태그로 사용 권장되지 않을 때 대체해서 사용할 수 있는 메소드나 클래스를 제시할 때 사용한다.
  - 문법 구조 : @see (패키지#생성자 or 필드 or 메소드명)<br><br>
- @author : 최초 작성자를 설명한다.
  - 문법 구조 : @author (작성자)<br><br>
- @version : 해당 메소드의 버전을 설명한다.
  - 문법 구조 : @version (버전)<br><br>
- @since : 해당 메소드가 프로젝트에 추가된 버전을 지정한다.
  - 문법 구조 : @since (버전)<br><br>
- 이 밖에도 @link, @implSpec, {@code} 등등 더 많은 태그들이 있지만 주로 가장 많이 사용되는 태그 위주로 정리하였다.
