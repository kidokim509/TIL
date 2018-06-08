# read_csv() 사용시 인용부호 주의사항

옵션 값 중 quotechar='"'에 주목!
딜리미터 (csv라면 ,)를 필드 내용 중에 포함해야 한다면, quotechar로 감싸면 된다는 취지의 옵션.
문제는 정말 필드 값에 "이 포함되어있는 경우에도 그냥 "는 quotechar라 생각하고 무시함.
그러니까 csv 파일에 "가 있어도 pandas로 읽으면 "가 안보인다.
재미있는건 필드 값 중간에 있는 "는 무사한데, 필드 값이 "로 시작하는 것만 그렇다.
문서에 정확한 설명이 있지는 않은데, 아마도 필드 값 전체를 "로 감싸서 quote하라는 의도인 듯.
아무튼 이 옵션에 대한 별 인지 없이 사용하면 필드 값에 있는 "가 사라지는 경우를 볼 수 있다.
quoting = csv.QUOTE_NONE 옵션을 주면 이 기능을 끌 수 있다.