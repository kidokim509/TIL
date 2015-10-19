# Sqoop Tips

## mysql에서 direct mode로 import시 "mysqldump terminated with status 2" Error 발생

* Direct mode로 Sqoop Import할 때, DB가 MySQL이라면 Sqoop은 mysqldump를 실행함
* TaskTracker에 설치된 mysqldump의 버전이 mysql 서버와 compatible한지 점검해볼 것

## mysql에서 direct mode로 import시 mysqldump의 option
* --skip-opt
* --compact
* --no-create-db
* --no-create-info
* --quick // no buffering
* __--single-transaction__ // dump without lock with InnoDB

