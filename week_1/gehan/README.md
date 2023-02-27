# 스프링 스터디

1. 객체 지향에서 무조건적인 상속은 문제를 유발할 수 있음
2. DI : 구체적인것에 의존하지 말고, 추상적인 것(인터페이스)에 의존하자
3. OCP 법칙 : 바꾸면 안되고, 확장은 되어야 한다?
4. 람다 : 자바의 메서드를 함수처럼 사용 
5. 함수와 메서드의 차이점 확인
6. 
7. 

이 문서는 스프링 스터디에서 다루는 주제들을 나열하고 있습니다. 상속의 문제점, DI, OCP 법칙, 람다, 함수와 메서드의 차이점 등이 다루어질 예정입니다.

---

### 2.24(금) 스프링 MVC 1편 강의 정리

0. 시작하며

1. 백엔드 기술의 역사가 길기 때문에 새로운 기술에 대한 맥락 이해 부족
2. 스프링의 MVC 구조를 이해해야

1. 웹 서버, 웹 애플리케이션 서버
- 웹 서버 = 정적 리소스 파일 제공
- 웹 애플리케이션 서버(WAS) = 프로그램 코드를 실행하여 애플리케이션 로직 수행
    
    → 사실 둘 의 용어도, 경계도 모호함
    
    → WAS는 애플리케이션 코드를 실행하는데 더 특화
    

1. 웹 시스템 구성
- 이론상 WAS, DB만으로 서비스 구성 가능
    
    → 서버 과부화의 우려가 있음
    
    → 애플리케이션 로직 > 정적 리소스 : 상대적인 비용에서 큰 차이가 있음
    
    → WAS 장애시 기본 정적 리소스도 처리 못해 오류 메시지도 노출하지 못하는 문제
    
- 클라이언트 → 웹서버 → WAS → DB
    
    → 필요시에만 WAS에 접근
    
    → WAS 장애시 WEB 서버가 오류 화면 제공 가능 
    
1. 서블릿
- urlPatterns (ex. /hello)의 url이 호출되면 서블릿 코드가 실행
- http 요청 정보를 편리하게 이용할 수 있는 HttpServletRequest
- http 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
    
    [정리 - http 요청시]
    
    1. WAS는 Request, Response 객체를 새로 만들어서 서블릿 객체 호출
    2. 개발자는 Request 객체에서 HTTP 요청을 편리하게 꺼내서 사용
    3. Response 객체에서 HTTP 응답 정보를 편리하게 입력
    4. WAS는 Responss 객체에 담겨있는 내용으로 HTTP 응답 정보를 생성

1. 서블릿 컨테이너
- 서블릿의 생성, 초기화, 호출, 종료등의 생명주기 관리
- 서블릿 객체는 싱글톤으로 관리
    
    → 요청 때마다 객체를 생성하는 것은 비효율적, 모든 요청은 동일한 서블릿 객체 인스턴스에 접근
    
- 동시 요청을 위한 멀티 쓰레드 처리 지원

1. 동시 요청 - 멀티 쓰레드
- 서블릿 객체를 누가 호출하는가? → 스레드
- 동시 요청시 생기는 문제점 해결 방법은?
    1. 요청 마다 쓰레드 생성
        
        단점 1 : 쓰레드 생성 비용이 매우 비쌈
        
        단점 2 : 쓰레드는 컨텍스트 스위칭 비용이 발생함
        
        단점 3 : 쓰레드 생성에 제한이 없어 cpu, 메모리 임계점을 넘어 서버가 죽을 수 있음
        
    2. 스레드 풀 생성
        - 쓰레드가 필요하면 이미 생성되어 있는 쓰레드를 쓰레드 풀에서 꺼내서 사용
        - 쓰레드 풀에 생성 가능한 쓰레드의 최대치를 관리
        
        → 요청마다 쓰레드 생성하는 방식의 단점 보완
        
        → 실무에서도 맥스 쓰레드의 세팅에 따라 성능에 큰 차이를 보임
        
        ⇒ 동시에 처리하는 부분에 대한 설명은 부족한 것 아닌가?
        
- 멀티 쓰레드에 대한 부분은 WAS가 처리해줌
- 멀티 쓰레드 환경이므로 싱글톤 객체(서블릿, 스프링 빈)들을 주의해서 사용!
    
    
1. HTML, HTTP API, CSR, SSR
- 정적 리소스
- HTML 페이지 : WAS가 데이터베이스에서 데이터 조회 후 동적으로 HTML 생성 → 웹 브라우저로
- HTTP API : 요청 → WAS에서 JSON을 반환
    - 데이터만 주고 받음, UI가 필요할 시 클라이언트가 별도 처리
    - 앱, 웹 클라이언트, 서버 to 서버

 백엔드 개발자가 고민해야 하는 세 가지!!

- (SSR)서버 사이드 렌더링 :
    
    서버에서 최종 HTML을 생성해서 클라이언트에 전달
    
    → 주로 정적인 화면에 사용
    
    관련기술 : JSP, 타임리프
    
- (CSR)클라이언트 사이드 렌더링 :
    
    HTML 결과를 자바스크립트를 사용해서 웹 브라우저에 동적으로 생성
    
    → 주로 동적인 화면에 사용, 웹 환경을 앱처럼 필요한 부분부분 변경할 수 있음
    
    관련기술 : React, Vue.js ⇒ 웹 프론트 영역
    
1. 자바 웹 기술 역사
- 서블릿
- JSP
- 서블릿, JSP 조합 MVC 패턴 사용
- MVC 프레임워크 춘추 전국 시대
- 애노테이션 긴반의 스프링 MVC 등장
- 스프링 부트의 등장
- 최신 기술 - 스프링 웹 기술의 분화
    - Web Servlet = Spring MVC
    - Web Reactive = Spring WebFlux
        
        특징
        
        1. 최소 쓰레드로 최대 성능 - 스레드 컨텍스트 스위칭 비용 효율화
        2. 함수형 스타일로 개발 - 동시처리 코드 효율화
        3. 서블릿 기술 사용 X
        
        But.
        
        1. 웹 플럭스는 기술적 난이도가 매우 높음
        2. 아직은 RDB(관계형 DB) 지원 부족
        3. 일반 MVC의 쓰레드 모델도 충분히 빠름
        4. 실무에서 아직 많이 쓰이지 않음