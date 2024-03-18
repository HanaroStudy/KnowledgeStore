# 1-6. HTTP 헤더 정보

- 일반정보
    - From : 유저 에이전트의 이메일 정보
        - 일반적으로 잘 사용되지는 않음
        - 검색 엔진 같은 곳에서 주로 사용
        - 요청에서 사용
    - Referer : 이전 웹 페이지 주소
        - 현재 요청된 페이지의 이전 웹 페이지 주소
        - A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
        - 요청에서 사용
    - User-Agent : 유저 에이전트 애플리케이션 정보
        - 클라이언트의 애플리케이션 정보(웹 브라우저의 정보 등등)
        - 통계 정보
        - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
        - 요청에서 사용
    - Server : 요청을 처리하는 오리진 서버의 소프트웨어 정보
        - Server: Apache/2.2.22(Debian)
        - server: nginx
        - 응답에서 사용
    - Date : 메시지가 생성된 날짜

- 특별한 정보
    - Host : 요청한 호스트 정보(도메인)
        - 요청에서 사용
        - 하나의 서버가 여러 도메인을 처리해야 할 때
        - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
    - Location : 페이지 리다이렉션
        - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동(리다이렉트)
        - 응답코드 3xx에서 설명
        - 201(Created) : Location 값은 요청에 의해 생성된 리소스 URI
        - 3xx(Redirection) : Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킴
    - Allow : 허용 가능한 HTTP 메서드
        - 405(Method Not Allowed)에서 응답에 포함해야함
        - Allow: GET, HEAD, PUT
    - Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
        - 503(Service Unavailalbe) : 서비스가 언제까지 불능인지 알려줄 수 있음
        
        ```
        Retry-After: Fri, 31 Dec 1999 23:59:59 GMT(날짜 표기)
        Retry-After: 120(초단위 표기)
        ```
        

- 인증
    - Authorization : 클라이언트 인증 정보를 서버에 전달
        - Authorization : Basic xxxxxxxxxx
    - WWW-Authenticate : 리소스 접근 시 필요한 인증 방법 정의
        - 401 Unauthorized 응답과 함께 사용
        
        ```
        WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"",
        Basic realm="simple"
        ```
        

- 쿠키
    - Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
    - Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고 HTTP 요청시 서버로 전달
    
    ```
    set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT;
    						path=/; domain=.google.com; Secure
    ```
    
    - 사용처
        - 사용자 로그인 세션 관리, 광고 정보 트래킹
            - 쿠키 정보가 항상 서버에 전송되기 때문에 네트워크 트래픽이 추가로 유발됨 → 최소한의 정보(세션 id, 인증 토큰)만 사용 → 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 localStorage, sessionStorage
    - 생명주기
        - expires
        - max-age : 0이나 음수를 지정하면 쿠키 삭제
        - 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
        - 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지
    - 도메인
        - 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
            - domain=example.org를 지정해서 쿠키 생성
            - example.org, dev.example.org도 쿠키 접근
        - 생략 : 현재 문서 기준 도메인만 적용
            - example.org에서 쿠키를 생성하고 domain 지정을 생략
            - example.org에서만 쿠키 접근. dev.example.org에서는 쿠키 미접근
    - 경로
        - 일반적으로 path=/ 루트로 지정
        - path=/home
        - /hello → 불가능
    - 보안
        - Secure
            - 쿠키는 http, https를 구분하지 않고 전송하는데 Secure를 적용하면 https인 경우에만 전송
        - HttpOnly
            - XSS(Cross Site Scripting) - 게시판이나 웹 메일 등에 자바 스크립트와 같은 스크립트 코드를 삽입해 개발자가 고려하지 않은 기능이 작동하게 하는 치명적일 수 있는 공격
            - XSS 공격 방지
            - 자바스크립트에서 접근 불가. HTTP 전송에만 사용
        - SameSite
            - XSRF(Cross-Site Request Forgery) - 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 하는 공격
            - XSRF 공격 방지. 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송