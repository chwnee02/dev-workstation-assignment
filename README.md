# 개발 워크스테이션 구축 과제

## 1. 과제 개요
- 과제 목적: 개발 워크스테이션 구축

## 2. 작업 환경
- 사용한 운영체제: macOS
- 사용한 터미널: zsh in VS Code
- 사용한 도구: VS Code, Git, Docker Desktop
- Git version: 2.56.0
- Docker version: 29.9.2, build a82e528

## 3. 수행 항목 체크리스트
• CLI 기본 실습
	• 현재 위치 확인
	• 파일 목록 확인
	• 숨김 파일 확인
	• 파일/폴더 생성 및 이동
		• 폴더 생성
		• 폴더 이동
		• 파일 생성
		• 파일 복사
		• 위치 이동
		• 파일/폴더 이름 변경
		• 파일/폴더 삭제
		• 파일 내용 확인
• 파일 권한 실습
	• 파일 권한 확인
	• chmod 실습
		• 파일 권한 변경
		• 디렉터리 권한 변경
• Docker 설치 및 기본 점검
	• 버전 확인
	• 실행 상태 확인
	• 이미지 다운로드 및 확인
	• 컨테이터 실행/중지
	• 컨테이너 목록 확인
	• 컨테이너 로그, 리소스 확인
• 컨테이너 실행 실습
	• hello-world 실행
	• ubuntu 컨테이너 실행
	• exec/attach 차이
• 포트 매핑
• 바인드 마운트 및 볼륨
	• 바인드 마운트 실행
	• docker 볼륨 실행
• dockerfile 작성
	• dockerfile 작성
	• 이미지 빌드
	• 컨테이너 실행
• git/github 연동
• 트러블슈팅	
	
## 4. CLI 기본 실습
- 현재 위치 확인: pwd
	/Users/chwn/dev/dev-workstation-assignment
- 파일 목록 확인: ls
	README.md       screenshots
- 숨김 파일 확인: ls -la
- 파일/폴더 생성 및 이동:
	- 폴더 생성: mkdir -p ~/dev/dev-workstation-assignment
	- 이동: cd ~/dev/dev-workstation-assignment
	- 파일 생성: touch README.md
	- 파일 복사: cp test.md test_copy.md
	- 파일 이름 변경: mv test_copy.md test2.md
	- 파일 삭제: rm test2.md
	- 파일 내용 확인: cat .gitignore

## 5. 권한 실습
- 파일 권한 확인: ls -l
- chmod 실습:
	- 파일 권한 변경: chmod 644 text.txt
	- 디렉토리 권한 변경: chmod 755 test-folder

## 6. Docker 기본 실습
- 버전 확인: docker --version
- 상태 정보 확인: docker info
- 이미지 다운로드 및 확인:
	- 이미지 다운로드: docker pull nginx
	- 이미지 목록 보기: docker images
	- 실행: docker run -d --name nginx-practice -p 8080:80 nginx
	- 상태 확인: docker ps, docker ps -a
	- 로그 확인: docker logs nginx-practice
	- 리소스 확인: docker stats stats-practice
	- 중지:docker stop nginx-practice
	
## 7. 컨테이너 실행 실습:
	- hello-world 실행: docker run hello-world	
	- ubuntu 컨테이너 실행: 
		- 이미지 다운: docker pull ubuntu
		- 실행 및 내부 진입: docker run -it --name ubuntu-practice ubuntu
		- 내부 명령어: ls, echo "Hello Ubuntu", pwd, whoami
		- 종료: exit
	- exec / attach 차이 관찰:
		- exec 실습:
			docker exec -it ubuntu-session bash
			whoami
			pwd
			echo "Hello from exec"
			exit
		- attach 실습:
			docker attach ubuntu-session
			whoami
			pwd
			echo "Hello from attach"
			Ctrl + P
			Ctrl + Q
		- 컨테이너 종료
			docker stop ubuntu-session
		- exec / attach 차이
			- exec는 실행 중인 컨테이너 안에서 새로운 프로세스를 추가로 실행하는 것
			- attach는 컨테이너가 처음 시작될 때 실행한 메인 프로세스에 연결(접속)하는 것
			- exec는 새로운 작업을 시작하고 싶을 때, attach는 메인 프로세스의 입출력을 확인하고 싶을 때 사용

## 8. 포트 매핑
- 실행 명령: docker run -d --name web-test -p 8080:80 nginx
- 브라우저 접속 결과: nginx 기본 환영 페이지(Welcome to nginx!) 정상 출력 확인
- 주소창 확인: http://localhost:8080 

## 9. 바인드 마운트와 볼륨
### 바인드 마운트
- 명령어: docker run -d --name web-mount -p 8081:80 -v "$(pwd)/site:/usr/share/nginx/html:ro" nginx
- 호스트-컨테이너 동기화 결과: 호스트의 `site` 폴더와 컨테이너의 `/usr/share/nginx/html` 경로가 연결되어, 호스트에서 작성한 `index.html` 내용이 브라우저에 정상적으로 출력되는 것을 확인함.

### 볼륨
- 명령어: docker run -d --name web-volume-test -p 8082:80m-v web-volume:/usr/share/nginx/html nginx
- 호스트-컨테이너 데이터 공유 결과: Docker 볼륨을 컨테이너의 웹 페이지 경로에 연결하고, 컨테이너 내부에서 작성한 파일이 정상적으로 출력되는 것을 확인함.
- 컨테이너 삭제 후 데이터 유지 여부: 기존 컨테이너를 삭제한 후 동일한 볼륨을 새로운 컨테이너에 연결했을 때 기존 데이터가 유지되는 것을 확인함.

## 10. Dockerfile 실습
- Dockerfile 작성: 
	FROM nginx
	COPY site/ /usr/share/nginx/html/
- 이미지 빌드: docker build -t my-nginx-page .
- 컨테이너 실행: docker run -d --name my-page-container -p 8083:80 my-nginx-page

## 11. Git / GitHub 연동
- git 사용자 정보 확인:
	git config --global user.name
	git config --global user.email
- 기본 브랜치 설정: git branch -M main
- git 초기화: git init
- 파일 스테이징: git add .
- 커밋 실행: git commit -m "Initial project setup"
- 원격 저장소 등록: git remote add origin https://github.com/chwnee02/dev-workstation-assignment.git
- push 실행: git push -u origin main

## 12. 트러블슈팅
### 문제 1
- 증상:
	- docker attach ubuntu-session 명령어로 Ubuntu 컨테이너에 접속한 후 Ctrl + P → Ctrl + Q를 눌러도 컨테이너에서 빠져나오지 못하는 문제가 발생했다.
	- Ctrl + P 입력 시 VS Code 터미널에서 이전 명령어가 불러와지는 등 원하는 동작이 실행되지 않았다.
- 원인:
	- VS Code 터미널에서 Docker의 기본 detach 단축키인 Ctrl + P → Ctrl + Q가 의도한 대로 동작하지 않았다.
	- 컨테이너 자체의 문제인지 확인하기 위해 docker inspect 명령어로 설정을 확인했으며, Tty와 OpenStdin 값이 모두 true로 정상 설정되어 있음을 확인했다.
- 해결:
기본 detach 키 대신 Ctrl + X를 사용하도록 변경하여 해결했다.
docker attach --detach-keys="ctrl-x" ubuntu-session
docker attach: 실행 중인 컨테이너의 메인 프로세스에 연결한다.
--detach-keys="ctrl-x": 컨테이너를 종료하지 않고 연결을 해제하는 키를 Ctrl + X로 지정한다.
ubuntu-session: 연결할 컨테이너의 이름이다.
명령어 실행 후 Ctrl + X를 눌러 연결을 해제했다. 이후 다음 명령어로 컨테이너 상태를 확인했다.
docker ps
ubuntu-session의 상태가 Up으로 표시되는 것을 확인하여 컨테이너를 종료하지 않고 연결 해제하는 데 성공했다.

### 문제 2
- 증상:
	- git push -u origin main 명령어 실행 시 Push가 거부되는 오류가 발생했다.
	- Updates were rejected because the remote contains work that you do not have locally.라는 메시지가 출력되었다.
- 원인:
	- GitHub 원격 저장소에 로컬 저장소에는 없는 커밋이나 변경 사항이 존재했다.
	- 원격 저장소와 로컬 저장소의 이력이 일치하지 않아 Push가 거부되었다.
- 해결:
	- git pull origin main --allow-unrelated-histories --no-rebase 명령어를 실행하여 원격 저장소의 변경 사항을 가져오고 로컬 저장소와 병합했다.
	- Merge made by the 'ort' strategy. 메시지를 통해 병합이 성공한 것을 확인했다.
	- 이후 git push -u origin main 명령어를 다시 실행하여 원격 저장소에 업로드했다.

## 13. 과제 목표 설명
Q1. 절대 경로와 상대 경로의 차이를 예시를 들어 설명
A1. 

Q2. 파일 권한의 의미(r/w/x와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명)
A2. 
- 파일 권한 r/w/x의 의미
	- r: read 읽기
	- w: write 쓰기
	- x: execute 실행
	- 숫자 표기의 의미
	각 권한마다 정해진 숫자가 있다. r: 4, w: 2, x:1처럼 정해진 숫자가 있고, 필요한 숫자를 더해서 권한을 표현한다.
	ex) rwx = 4+2+1 = 7, rw- = 4+2 =6 
	755, 644 같은 숫자 세 자리는 각각 사용자가 실행할 수 있는 권한의 종류를 의미
	755의 경우 7 = 소유자(rwx), 5 = 그룹(r-x) 5 = 기타사용자(r-x)
	644의 경우 6 = 소유자(rw-), 4 = 그룹(r--), 4 = 기타사용자(r--)

Q3. 기존 Dockerfile을 기반으로 "커스텀이미지"를 만들 수 있다
A3. 

Q4. 포트 매핑이 필요한 이유를 설명할 수 있다
A4.

Q5. Docker 볼륨(영속 데이터)을 설명할 수 있다
A5. 

Q6. git과 github의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다