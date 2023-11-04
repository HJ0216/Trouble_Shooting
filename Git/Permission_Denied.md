## Permission denied (publickey)

### Problem
```Bash
git@github.com:HJ0216/Trouble_Shooting.git
```

<br/>

### Cause of Problem
SSH 프로토콜을 사용한 clone 방식에서 올바른 SSH가 존재하지 않음

<br/>

### Solution
1. HTTP 방식 Clone
```Bash
https://github.com/HJ0216/Trouble_Shooting.git
```
* SSH Key가 아닌 Username과 Password가 필요

<br/>

2. SSH 방식 Clone
```Bash
# 1. 이전에 생성된 Key가 있는지 확인
# 루트 디렉토리 아래의 .ssh 폴더의 id_rsa.pub이라는 파일 조회
# id_rsa.pub: 서버에 저장되는 공개키
cat ~/.ssh/id_rsa.pub

# 2. SSH key가 없을 경우, SSH key 생성
ssh-keygen
# 이후 종료 시까지 Enter

# C:\Users\user\.ssh에서 private key(id_rsa), public key(id_rsa.pub) 확인

# 3. SSH key가 있을 경우, SSH key 등록
# https://github.com/settings/keys
# New SSH key 버튼 클릭
# Title: 이름, Key: id_rsa.pub 입력 후, Add SSH Key 버튼 클릭

```
* 통신할 때 ID, PWD 대신 SSH 공개 Key 전송
    * SSH Key를 생성하면 2개의 키가 한 쌍(Public-Private)으로 생성
    * SSH 통신을 할때 클라이언트에서 생성된 공개키를 통신하고자하는 서버에 저장
    * 이후 클라이언트가 서버에 통신을 시도할때 서버에 저장된 공개키가 클라이언트 로컬에 저장된 비공개키와 한쌍임을 확인하고 안전한 통신채널을 확립

<br/>

### 📚 참고 자료
[Github SSH Key 등록하기](https://velog.io/@skyepodium/Github-SSH-Key-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B8%B0)
