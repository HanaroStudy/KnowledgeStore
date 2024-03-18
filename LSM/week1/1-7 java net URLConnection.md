# 1-7. java.net.URLConnection

- Creating a connection to a URL
    1. opeenConnection - URL에서 connection 객체 생성
        
        ```java
        transient URLStreamHandler handler;
        
        public URLConnection openConnection() throws java.io.IOException {
        		return handler.openConnection(this);
        }
        ```
        
        - URL에 대한 프로토콜 핸들러의 `URLStreamHandler.openConnection(URL)` 메소드를 호출할 때마다 URLConnection의 새 인스턴스가 생성됨
        - URLConnection 인스턴스는 생성 시 실제 네트워크 연결을 설정하지 않으며, `URLConnection.connect()`를 호출할 때만 발생
        - HTTP의 경우 HttpURLConnection이 반환되고 JAR의 경우 JarURLConnection이 반환
        - 추가 - openConnection(Proxy proxy)
            
            > 지정된 프록시를 통해 연결이 이루어진다는 점을 제외하면 openConnection()과 동일. 프록시를 지원하지 않는 프로토콜 핸들러는 프록시 매개변수를 무시하고 정상적인 연결을 생성. 기본 ProxySelector 설정이 선점됨.
            > 
            
            ```java
            public URLConnection openConnection(Proxy proxy) throws java.io.IOException {
                    if (proxy == null) {
                        throw new IllegalArgumentException("proxy can not be null");
                    }
            
                    // Create a copy of Proxy as a security measure
                    Proxy p = proxy == Proxy.NO_PROXY ? Proxy.NO_PROXY : sun.net.ApplicationProxy.create(proxy);
                    @SuppressWarnings("removal")
                    SecurityManager sm = System.getSecurityManager();
                    if (p.type() != Proxy.Type.DIRECT && sm != null) {
                        InetSocketAddress epoint = (InetSocketAddress) p.address();
                        if (epoint.isUnresolved())
                            sm.checkConnect(epoint.getHostName(), epoint.getPort());
                        else
                            sm.checkConnect(epoint.getAddress().getHostAddress(),
                                            epoint.getPort());
                    }
                    return handler.openConnection(this, p);
            }
            ```
            
    
    1. 설정 매개변수와 general request properties 조작
    
    1. remote 객체에 대한 실제 연결이 이루어지면 connect 메서드 사용
        - connect
            
            ```java
            public abstract void connect() throws IOExcpetion;
            ```
            
            - 연결이 아직 설정되지 않았다면 URL이 참조하는 리소스에 대한 통신 링크를 연다
            - 연결이 이미 열려있는 상태에서 connect 메서드가 호출되면 호출이 무시됨
            - 생성 후 연결되기 전에 다양한 옵션 지정 가능 → doInput, Usecaches
            - getContentLength와 같이 연결에 의존하는 작업은 필요한 경우 암시적으로 연결 수행
        
    2. remote 객체가 사용 가능한 상태가 되면 헤더 필드와 remote 객체의 contents에 접근 가능
    
    1. set method
        - setAllowUserInteraction
            - true → 이 URL은 인증 대화 상자 팝업과 같은 사용자 상호 작용을 허용
            - false → 상호 작용 허용 X
        - setDoInput
            - DoInput → 애플리케이션(application)이 URL 연결에서 데이터 읽기
        - setDoOutput
            - DoOutput → 애플리케이션(application)이 URL 연결에서 데이터 쓰
        - setIfModifiedSince
        - setUseCaches
            - true → 가능할 때마다 캐싱 사용 가능
            - false → 항상 개체의 새로운 복사본을 얻으려고 시도해야 함
            
            ```java
            public void setUseCaches(boolean usecaches) {
                    checkConnected();
                    useCaches = usecaches;
            }
            ```
            
            - 프로토콜 별로 setDefaultUseCaches(String, boolean)을 사용해 재정의 될 수 있음
            
            ```java
            public void setDefaultUseCaches(boolean defaultusecaches) {
                    defaultUseCaches = defaultusecaches;
            }
            
            public static void setDefaultUseCaches(String protocol, boolean defaultVal) {
                    protocol = protocol.toLowerCase(Locale.US);
                    defaultCaching.put(protocol, defaultVal);
            }
            ```
            
        - setRequestProperty
            - 일반 요청 속성 설정
            - 해당 키가 있는 속성이 이미 존재하는 경우 해당 값을 새 값으로 덮어씀
            
            ```java
            public void setRequestProperty(String key, String value) {
                    checkConnected();
                    if (key == null)
                        throw new NullPointerException ("key is null");
            
                    if (requests == null)
                        requests = new MessageHeader();
            
                    requests.set(key, value);
            }
            ```
            
    
    1. Get method
        - getContent
        - getHeaderField
        - getInputStream
        - getOutputStream
        - getContetntEncoding
        - getContentLength
        - getContentType
        - getDate
        - getExpiration
        - getLastModified