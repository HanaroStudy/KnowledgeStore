# 1-3. URI

- URI(Uniform Resource Identifier)
    
    > Uniform : 리소스를 식별하는 통일된 양식
    > 
    > 
    > Resource : 자원, URI로 식별할 수 있는 모든 것 (제한 x)
    > 
    > Identifier : 다른 항목과 구분하는데 필요한 정보
    > 
    - URL(Locator) : 리소스가 있는 위치를 지정
        
        > scheme://[userinfo@]host[:port][/path][?query][#fragment]
        > 
        > 
        > https://www.google.com:443/search?q=hello&hl=ko
        > 
        - userinfo : URL에 사용자 정보를 포함해서 인증. 거의 사용하지 않음
        - port : 접속 포트. 일반적으로는 생략. 생략시 http는 80, https는 443
        - query : key=value 형태. ?로 시작하고 &로 추가 가능
    - URN(Name) : 리소스에 이름 부여
        
        > urn:isbn:8960777731
        > 
        - 리소스 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음