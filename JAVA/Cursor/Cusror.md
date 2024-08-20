# Cursor
- 기존 엑셀 추출 시 가장 많이 사용되는 PoiExcelUtil 라이브러리 대신 메모리 부담을 줄이고 추출 성능도 개선할 수 있는 Mybatis의 Cousor 객체를 설명하고 PoiExcelUtil의 문제점에 대해 정리해보려 한다.<br><br>

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

## DAO에 Cursor 적용하기
````
// 수정 이전
@Mapper
public interface excelDAO {
  public List<LinkedHashMap<String, Object>> selectExcel(ExcelVO vo);
}

// Cursor로 변경
import org.apache.ibatis.cursor.Cursor;

@Mapper
public interface excelDAO {
  public Cursor<LinkedHashMap<String, Object>> selectExcel(ExcelVO vo);
}
````
- 이걸로 DAO 수정은 끝! ~~참 쉽죠?🖌️~~