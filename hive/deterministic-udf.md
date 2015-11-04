# Deterministic / Non-deterministic	

**NOTE**: Information taken from [HIVE-9630](https://issues.apache.org/jira/browse/HIVE-9630)

Hive 쿼리를 짤 때, WHERE 절에서 어제 일자 데이터만 뽑아내려고 한다고 하자.

이 때 WHERE 절을 "dt = from_unixtime(unix_timestamp()-1*60*60*24, 'yyyyMMdd')" 같이 쓰면 테이블의 전체 파티션에 대한 Full Scan이 발생한다.

unix_timestamp()는 Non-deterministic 함수이므로 데이터를 가져오기 전에 호출되어 dt = '20151101' 과 같이 Scan할 파티션을 결정하는 것이 아니라, Record를 가져와서 비교할 때마다 계속 호출된다.

이러한 문제 때문에 Hive 1.2.0부터는 Deterministic 함수인 CURRENT_DATE(), CURRENT_TIMESTAMP()를 제공한다.
