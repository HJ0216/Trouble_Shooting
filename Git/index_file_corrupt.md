## index file corrupt

### Problem

```bash
error: bad signature 0x00000000
fatal: index file corrupt

```

<br/>



### Cause of Problem
Git 저장소의 인덱스 파일이 손상되었거나 부적절한 상태에 있을 때 발생하는 오류  
it은 작업 디렉터리의 변경 내용을 추적하기 위해 인덱스 파일을 사용하는데, 이 파일이 손상되면 Git 명령어를 실행할 때 문제가 발생  
- 디스크에 문제가 발생하거나 파일 시스템이 손상되었을 때 인덱스 파일이 손상될 수 있음
-  Git 명령어 중간에 오류가 발생하거나 비정상적으로 종료되면 인덱스 파일이 손상될 수 있음
- 디스크나 메모리 등의 하드웨어 문제로 인해 파일이 손상될 수 있음

  \+ git 조작하다가 갑자기 BIOS 업데이트가 진행되더니 나타난 2024 첫 에러😮

<br/>



### Solution
index 파일을 삭제하고 reset을 통해 인덱스를 마지막 커밋 버전으로 복원  
1. On OSX/Linux/Windows(With Git bash)

```bash
rm -f .git/index
git reset

```

2. On Windows (with CMD and not git bash)

```bash
del .git\index
git reset

```

<br/>



### 📚 참고 자료
[How to resolve "Error: bad index – Fatal: index file corrupt" when using Git](https://stackoverflow.com/questions/1115854/how-to-resolve-error-bad-index-fatal-index-file-corrupt-when-using-git)  
[[git] index file 문제 및 복원](https://loshy244110.medium.com/git-index-file-%EB%AC%B8%EC%A0%9C-%EB%B0%8F-%EB%B3%B5%EC%9B%90-20f47332cb0d)