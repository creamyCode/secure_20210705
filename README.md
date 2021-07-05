# secure_20210705

# 웹 취약점 관련 RMS에 반영될 항목 3가지 예시

### 1. Broken Authentication
취약한 인증으로 공격자가 수동 및 자동화된 방법으로 시스템에서 원하는 계정을 제어할 수 있는 취약점

##### 방지방법

* 관리자 로그인 기능 제한 
	* ip접근제어 
* 유추 가능한 관리자 이름 금지
	* 금지 키워드로 설정필요 (rms 설정필요)
* brute force
	* 일정 횟수 이상의 login 실패시 계정이 잠기거나 정해진 시간이후에 시도 가능한 login throttling 기능등을 적용 (rms 설정필요)

### 2. CSRF(Cross Site Request Forgery)
사이트간 요청 위조로, 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격방법

##### 공격방법 예시
```
...
	<img src="http://auction.com/changeUserAcoount?id=admin&password=admin" width="0" height="0">
...
```
1. 옥션 관리자 중 한명이 관리 권한을 가지고 회사내에서 작업을 하던 중 메일을 조회한다. (로그인이 이미 되어있다고 가정하면 관리자로서의 유효한 쿠키를 갖고있음)
2. 해커는 위와 같이 태그가 들어간 코드가 담긴 이메일을 보낸다. 관리자는 이미지 크기가 0이므로 전혀 알지 못한다.
3. 피해자가 이메일을 열어볼 때, 이미지 파일을 받아오기 위해 URL이 열린다.
4. 해커가 원하는 대로 관리자의 계정이 id와 pw 모두 admin인 계정으로 변경된다.


##### 방지방법

* referrer 검증
	* request header에 있는 요청을 한 페이지의 정보가 담긴 referrer 속성을 검증하여 차단
* csrf token 검증
	* 랜덤한 수를 사용자의 세션에 저장하여 사용자의 모든 요청(Request)에 대하여 서버단에서 검증하는 방법.

### 3. 부적절한 자원 해제
프로그램의 자원 예를 들면 열린 파일디스크립터 (Open File Descriptor), 힙 메모리 (Heap Memory) 소켓 ( 등은 유한한 자원이다 이러한 자원을 할당받아 사용한 후 더 이상 사용하지 않는 경우에는 적절히 반환하여야 하는데 프로그램 오류 또는 에러로 사용이 끝난 자원을 반환하지 못하는 경우이다.

##### 방지방법
* try/catch문 내에 위의 파일오픈, 소켓등을 이용시 이를 닫는 구문을 작성해야함
* 컴포넌트내에서 Observable이용시 해당 컴포넌트가 destory 되는 시점에 구독한 observable을 해제하는 구문을 작성해야함

```
subscription = observable.subscribe(() => { ... })
.
.
.
destroy () {
  this.subscription.unsubscribe();
  this.subscription = null;
}
```

# 웹 취약점 점검 툴 (무료)

취약점 점검가능한 데모사이트 : http://zero.webappsecurity.com/

### 1. arachni
url : https://www.arachni-scanner.com/download/
실행방법 : https://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221409941106

### 2. owasp zap
url : https://www.zaproxy.org/download/
실행방법 : https://blog.naver.com/is_king/221552898200
※ 실행간 java 설치 필요


# 시큐어 코딩 관련 참고자료

1. 소프트웨어 보안약점 진단가이드
 
https://www.kisa.or.kr/public/laws/laws3_View.jsp?cPage=6&mode=view&p_No=259&b_No=259&d_No=52&ST=T&SV=

2. OWASP 2020 10대 취약점

https://www.lesstif.com/security/owasp-2020-10-91291830.html#OWASP202010%EB%8C%80%EC%B7%A8%EC%95%BD%EC%A0%90-A1.Injection(%EC%9D%B8%EC%A0%9D%EC%85%98)
