# Cursor
- 엑셀 추출 기능 구현 시 메모리 부담을 줄이고 추출 성능도 개선할 수 있는 Mybatis의 Cousor 객체에 대한 정리<br><br>

## PoiExcelUtil
- Google에 엑셀 추출을 검색하면 가장 많이 나오는 스테디셀러 방식이다.
- 하지만 데이터가 늘어날 경우의 몇 가지 문제점을 적어보자면<br>
  1. 데이터 추출 건수를 제한하려면 Mapper XML 파일 내 작성한 쿼리를 변경해주어야 하기 때문에 데이터 추출 건수 제한조건이 변경되면 관련 모든 쿼리를 찾아서 수정해야한다.
  2. 전체 데이터를 JVM의 메모리 상에 올린 후에야 엑셀파일 생성이 시작되기 때문에, 추출대상이 많을 시 최악의 경우 Out Of Memory 문제가 발생할 수도 있다.<br><br>
 
- 따라서 다음과 같은 포인트를 중심에 두고 문제를 해결하고자 한다.
1. 엑셀 추출 건수가 바뀌어도 기존 쿼리의 수정은 없어야한다.
2. 기존 비슷한 추출 속도이거나, 더 빨라져야한다.
3. 엑셀 파일 작성 시 메모리 소비를 줄여아한다.
<br>

## 성능 튜닝
- fetchSize 설정인 **defaultFetchSize(전역) / \<select fetchSize="?">(XML)** 을 설정한다.
<br>

## 성능 튜닝 원리
- Cursor를 사용할 경우 기본 fetchSize를 이용하여 10개의 행을 받아오고, 이 10건을 커서를 통해 한 건씩 처리한 뒤 다 떨어지면 다시 10개 행을 요청하는 방식으로 동작한다.
- 만약 WAS 서버의 성능이 좋아서 데이터를 받아오는 시간보다 처리하는 시간이 짧다면, DB 서버에 추가 데이터를 요청하는 빈도가 높아질수록 성능에는 손해를 보게된다.<br>
✔️ 즉, Cursor가 미리 받아놓고 처리하는 데이터 양을 늘리면 성능이 개선될 수 있다.<br>
✔️ 또한, fetchSize가 작을 때에 비해 DB 연결을 빨리 반납해줄수 있는 장점도 있다. 이러한 방식을 Prefetch라고 한다.<br>

Mapper XML에서 fetchSize를 변경한 예시를 들어보면
````
<select id="selectExcel" parameterType="excelVO" fetchSize="1024" resultType="java.util.LinkedHashMap">
````
- 여기서 fetchSize는 벤치마크 및 충분한 테스트를 통해서 적정선으로 설정해주면 된다.
  - 무리해서 조절할 경우 안하느니 못한 성능이 나올 수 있으니 조심해야한다.
  - 또한, 만약 1만건 추출 제한을 걸고자하는데 fetchSize가 1만건 이상인 경우 이러한 기능의 이점이 없어지니 잘 생각하며 사용해야한다!
<br>

## 적용하기
### DAO
````
// 수정 이전
@Mapper
public interface excelDAO {
  public List<LinkedHashMap<String, Object>> selectExcel(ExcelVO vo);
}

// 수정 후
import org.apache.ibatis.cursor.Cursor;

@Mapper
public interface excelDAO {
  public Cursor<LinkedHashMap<String, Object>> selectExcel(ExcelVO vo);
}
````
<br>

### Service
- 우선, Cursor는 열려있는 커넥션을 기준으로 작동한다. 커넥션이 닫혀버리면 사용할 수 없게 되기 때문에 사용되는 도중에는 커넥션이 유지되어야한다.
- 현재 프레임워크 환경에서는 ServiceImpl 영역을 벗어나면 트랜잭션 처리를 종료하고 연결을 닫아 버린다.<br>
✔️ 즉, Controller 레이어까지 Cursor를 가져올 수 없기 때문에 Service 레이어에서 수정해야한다!<br>
````
// 수정 이전
public List<LinkedHashMap<String, Object>> exportExcel(ExcelVO vo);

// 수정 후
public Cursor<LinkedHashMap<String, Object>> exportExcel(HttpServletResponse response, ExcelVO vo);
````
<br>

### Controller
- 기존 PoiExcelUtil을 통해 구현된 엑셀 추출 대상 데이터 조회 및 다운로드 절차를 모두 Service 레이어로 위임하면 된다.
 ````
// 수정 이전
List<LinkedHashMap<String, Object>> excelData = excelSerive.exportExcel(excelVO);
PoiExcelUtil.exportExcel(response, "엑셀 추출", "엑셀 추출", excelData);

// 수정 후
excelSerive.exportExcel(response, excelVO);
````
<br>

### ServiceImpl
- 기존 Controller에서 수행했던 작업을 여기서 진행하면 된다. 즉, 2가지 작업을 수행하게 된다.
  1. 기존 DAO를 통해 엑셀 작성 대상 데이터를 모두 받는 과정을 데이터 조회용 Cursor로 받는걸로 대체
  2. PoiExcelUtil에서 Cursor에 대응하는 엑셀 다운로드 기능을 구현한 메소드 호출
 ````
// 수정 이전
@Transactional
@Override
public List<LinkedHashMap<String, Object>> exportExcel(ExcelVO vo) {
  return excelDAO.selectExcel(vo);
}

// 수정 후
@Transactional
@Override
public void exportExcel(HttpServletResponse response, ExcelVO vo) {
  Cursor<LinkedHashMap<String, Object>> excelData = excelDAO.selectExcel(vo);
  PoiExcelUtil.exportExcel(response, "엑셀 추출", "엑셀 추출", excelData, 10000); // 10000건 리미트
}
````
- 이걸로 수정은 끝! ~~참 쉽쥬?🖌️~~

## 🚨주의사항
- Cursor를 통해 처리하는 방법은 Cursor로 한 행 씩 접근하며 처리하기 때문에 튜닝하기 전에는 더 느려질 수 있어 테스트가 필요하다!!!!!
- 그리고 환경별 WAS가 다르기 때문에 안정성 문제가 있는지도 확인해야한다.
