1. JPA 관련
- entity ID 키 값에 따른 배치 문제
현재 hibernate의 경우 
Entity의 key가 AUTO또는 IDENTITY로 generate되는 경우( ex : @GeneratedValue(strategy =  GenerationType.IDENTITY or GenerationType.AUTO) ) 
JDBC level의 insert batching을 지원하지 않아 batch_size 설정이 쓸모 없음
batch가 필요할 경우 ID key를 @GeneratedValue(strategy =  GenerationType.TABLE)로 설정하면 설정 작동

property나 enum등으로 설정하여
for문 돌면서 일정 갯수 저장 후 flush하고 다시 저장을 반복하면됨

현재 35000건 정도의 데이터가 batch없이 작업할 경우 3분후반대가 소요되고
배치 작동시 40초내외 소요

- entity ID키가 autoincrement나 uuid같이 unique가 보장되지 않을 경우 insert
String값이 키일경우 saveAll에서 저장전 select문이 실행됨(30개면 30번의 connection과 select가 실행)
엄청난 부하가 야기됨

Save는 추후에 확인해야함