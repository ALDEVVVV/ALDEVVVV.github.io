# Memory 크기 및 Cache크기 확인



### 명령어

free -m



## 캐시 삭제 방법

1.  Clear PageCache only
   
   echo 1 > /proc/sys/vm/drop_caches

2.  Clear dentries and inodes
   
    echo 2 > /proc/sys/vm/drop_caches

3.  Clear PageCache , dentries and inodes

        echo 3 > /proc/sys/vm/drop_caches



PageCache만 지우는 것이 엔터프라이즈 및 프로덕션에서 가장 안전함

2,3번은 무엇을 지우는 지 모르면 지우지 않는게 좋음



### Page , dentries , inodes 캐시

Page = 물리적 저장/통신 장치와 데이터를 주고 받을 때 메모리에 적재 후 동일한 데이터에 접근 할 경우 메모리에서 바로 가져와 I/O 성능을 높이기 위하여 사용 Page라는 단위로 관리함



inode , dentry = 파일의 자료구조를 의미 보다 빠른 데이터 접근을 위해 Slab의 자료구조에 추가되어 사용되며 dentry는 경로명 탐색을 위한 cache 역할도 수행한다.
