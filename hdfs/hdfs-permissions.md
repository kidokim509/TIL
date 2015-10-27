# HDFS Permissions

## POSIX 모델
* 각 파일/디렉토리는 1개의 소유자(Owner)와 1개의 그룹(Group)을 갖는다.
* 각 파일/디렉토리는 소유자, 그룹, 그 외 사용자(Others) 각각에게 별도의 권한(R|W|X)을 준다.
* 파일
  * R: Read the file
  * W: Write or Append to the file
* 디렉토리
  * R: list the contents of the directory
  * W: create or delete files or directories
  * X: access a child of the directory
  * Sticky Bit: Superuser 또는 소유자만이 디렉토리를 삭제하거나 디렉토리 안의 파일을 이동할 수 있다.
* 주의
  * 디렉토리에 W 권한이 있는 사용자는 해당 디렉토리안의 파일/디렉토리를 삭제할 수 있다. (삭제하려는 파일/디렉토리에 W 권한이 없더라도.)
  * 디렉토리에 READ-EXECUTE(R and X) 권한이 모두 있는 사용자만이 디렉토리 안의 내용을 list할 수 있다.


## ACL (Access Control Lists)
* 파일/디렉토리에 복수개의 소유자, 그룹 권한을 설정하여 세밀한(fine-grained) 권한 관리를 할 수 있음
* hadoop fs -ls 명령 결과 화면에서 Permission 끝에 '+'가 붙은 파일/디렉토리는 ACL이 설정되어 있는 것임

