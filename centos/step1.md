# 리눅스 명령어

## 한국어 설정
root 권한으로 바꿉니다. 

`su -`{{execute}}

지역 시간대를 서울로 바꿉니다. 
`ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime`{{execute}}

한국어 로케일을 등록합니다. 

`localedef -f UTF-8 -i ko_KR ko_KR.UTF-8`{{execute}}

쉘의 LANG 환경 설정값을 한국어로 바꿉니다. 

`export LANG=ko_KR.UTF-8`{{execute}}

잘 반영되었는지 확인해 봅니다. 
아래 명령어를 실행했을 때 시간은 한국 시간으로 요일은 한글로 보여야합니다.

`date`{{execute}}



## ls
ls 명령어는 디렉터리의 내용을 보는 명령어입니다. 

`ls`{{execute}}

