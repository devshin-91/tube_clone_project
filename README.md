# tube-clone-project
유튜브 클론 프로젝트

## 1. 개요
- 장고 프레임워크에서 유튜브 기능을 구현하여 장고를 학습

## 2. 프로필
<table>
    <tr>
        <th>신찬수</th>
    </tr>
        <td><img src="./project_img/profile.jpg"  alt="프로필 사진"></td>
</table>

## 3. 개발환경
- 운영체제
    - Windows 10
- 에디터
    - VS Code
- 프레임워크
    - Django

## 4. Django 및 주요 라이브러리 버전
```
Django==5.0.2
pillow==10.2.0
```

## 5. 프로젝트 구조
|          기능           |                                URL                                 
| :---------------------: | :----------------------------------------------------------------: |
|     메인페이지     |                              /tube                                
|      회원가입      |                              /accounts/signup                          
|       로그인       |                              /accounts/login/                          
|      로그아웃      |                              /accounts/logout                          
|     프로필 조회    |                              /accounts/profile
|     게시물 조회    |                              /tube/int:pk
|       글 쓰기      |                              /tube/create
|   게시물 업데이트  |                              /tube/int:pk/update
|     게시물 삭제    |                              /tube/int:pk/delete
|     태그 별 조회   |                              /tube/tag/str:tag
|      댓글 삭제     |                              /tube/int:pk/comment_delete
|        구독        |                              /tube/int:post_id/int:user_id/subscribe
|      구독 취소     |                              /tube/int:post_id/int:user_id/unsubscribe
                                        
## 6. ER diagram
<img src="./project_img/ER diagram.jpg"  alt="ER diagram">

## 7. 시연
- 회원가입, 프로필 확인, 로그아웃

    <img src="./project_img/회원가입_로그아웃.gif"  alt="회원가입, 로그아웃">

- 중복가입 여부 확인, 로그인

    <img src="./project_img/중복가입_로그인.gif"  alt="중복가입 여부, 로그인">

- 게시물 생성, 수정, 삭제

    <img src="./project_img/게시물_생성,수정,삭제.gif"  alt="게시물_crud">

- 댓글 생성 및 삭제

    <img src="./project_img/댓글_생성,삭제.gif"  alt="댓글_생성,삭제">

- 구독 및 구독 취소 

    <img src="./project_img/구독_구독취소.gif"  alt="구독 및 구독취소"> 

## 8. 사용방법
0. (옵션) 기존 내용을 지우고 사용하고자 한다면
```
- db.sqlite3 파일 삭제
- .\media 하위 폴더 blog 삭제
```
1. 가상환경 생성 및 활성화
```cmd
> python -m venv venv
> .\venv\Scripts\activate # 리눅스 환경이라면, source venv/bin/activate
```
2. 필수 라이브러리 설치
```cmd
> pip install -r requirements.txt
```
3. DB 마이그레이션
```cmd
> python manage.py migrate
```
4. admin 계정 새로 생성 (기존 admin 계정: admin, 비밀번호: Admin12#$)
```cmd
> python manage.py createsuperuser
```
5. 장고 서버 실행
```cmd
> python manage.py runserver
```
6. 접속
```cmd
http://127.0.0.1:8000/tube/
```

# 9. 트러블슈팅
- admin 이외 가입자의 게시물 생성 불가 이슈
    - 문제 : 글쓰기 페이지에서 모든 항목 작성해서 제출해도 업로드가 초기화되면서 글쓰기 페이지에 남게 됨
    - 원인 파악 : 디버깅 결과, form이 유효하지 않아서 계속 글쓰기 페이지에 남게 됨. 
    - 해결 : ./tube/views.py 내 form을 정의하는 코드 부분에 FILE에 대한 내용이 누락. 이를 다음과 같이 수정함.
        ```python
        # ./tube/views.py line 61
        '''
        기존 : form = PostForm(request.POST)
        ->
        수정 : form = PostForm(request.POST, request.FILES)
        '''
        ```
    - 도움을 받은 참고 링크 : https://simpleisbetterthancomplex.com/tutorial/2016/08/01/how-to-upload-files-with-django.html

# 10. 기타 개발 수정 옵션
- 화이트리스트 : ./config/settings.py => ALLOWED_HOSTS 옵션에서 특정 IP 또는 특정 IP 대역을 지정할 수 있다.
- 서드파티 DB 연동 : ./config/settings.py => DATABASES 옵션에서 MySQL, PostgresSQL 등의 DB를 연동할 수 있다. (기본 DB: sqlite3)
- 배포 전환 시 오류 페이지 : ./config/settings.py => DEBUG = False => 페이지가 없거나 에러 발생시 디버그 화면이 나오지 않게 되며, 대신 대체 페이지를 설정하면 된다.
- 배포 목적이면, 안정성과 성능의 이유로 Nginx와 gunicorn을 사용하는 것을 권장한다.

 
