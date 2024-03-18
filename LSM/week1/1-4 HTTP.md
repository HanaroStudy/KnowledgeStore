# 1-4. HTTP

- HTTP 특징
    - 클라이언트-서버 구조
        - Request - Response 구조
        - 클라이언트는 서버에 요청을 보내고 응답을 대기
        - 서버가 요청에 대한 결과를 만들어서 응답
    - 무상태 프로토콜(stateless)
        - 서버가 클라이언트의 상태를 보존하지 않는다
        - 서버 확장성이 높지만 클라이언트가 추가 데이터를 전송해야한다
    - 비연결성
        - HTTP는 기본이 연결을 유지하지 않는 모델
        - 일반적으로 초 단위 이하의 빠른 속도로 응답
        - 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
        - 한계점 - TCP/IP 연결을 새로 맺어야 함. 웹 브라우저로 사이트를 요청하면 HTMl 뿐만 아니라 자바스크립트, css, 추가 이미지 등등 수많은 자원이 함께 다운로드 됨 → 지금은 지속 연결(Persistent Connections)로 해결
    - HTTP 메시지
        
        > start-line = request-line / status-line
        > 
        
        > request-line = <method> <request-target> <HTTP-version CRLF>
        > 
        
        ```jsx
        GET /search?q=hello&hl=ko HTTP/1.1
        ```
        
        > status-line = <HTTP-version> <status-code> <reason-phrase CRLF>
        > 
        
        ```jsx
        HTTP/1.1 200 OK
        ```
        
    - 단순함, 확장 가능