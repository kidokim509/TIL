# HDFS Snapshot Design

__NOTE__: Information taken from [1](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsSnapshots.html) [2](http://cdn.oreillystatic.com/en/assets/1/event/100/HDFS%20Snapshots%20and%20Beyond%20Presentation.pdf)

## The implementation of HDFS Snapshots is efficient
1. Snapshot creation is instantaneous: the cost is O(1) excluding the inode lookup time.
2. Additional memory is used only when modifications are made relative to a snapshot: memory usage is O(M), where M is the number of modified files/directories.
3. Blocks in datanodes are not copied: the snapshot files record the block list and the file size. There is no data copying.
4. Snapshots do not adversely affect regular HDFS operations: modifications are recorded in reverse chronological order so that the current data can be accessed directly. The snapshot data is computed by subtracting the modifications from the current data.

## 정리
* Copy-On-Write
  * Snapshot 생성을 한다고 해서 데이터 블록이 복제되는 것이 아니다.
  * Name Node는 Snapshot을 생성한 시점 이후의 디렉토리 및 파일 변경을 발생 역순으로 기록한다. (block list, file size)
* Snapshot = Current data - Modification
  * Current data는 바로 read
  * Snapshot 데이터를 읽을 때는 처리 시간 소요: Current data에서 Modification을 빼야함. (뺀다 = 마치 변경을 Undo 하듯, 실제로는 Block pointing)

