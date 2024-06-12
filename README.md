## HTTP(Hyper Text Transfer Protocol) Method 
- 클라이언트와 서버 사이에 이루어지는 요청(Request)과 응답(Request) 데이터를 전송하는 방식

### 주요 메서드
- **GET**, **POST**, **PUT**, **PATCH**, **DELETE**, HEAD, OPTIONS, CONNECT, TRACE

  <br />
  <hr />
  <br />

- **GET** : 데이터 조회시 주로 사용 <br />
   * 요청 전송시 Query String을 통해 전송
   * QueryString => url의 끝에 ' ? + 이름,값 ' 으로 구성된 요청 파라미터를 의미
   * 요청 파라미터가 여러개일 경우 & 로 연결
    ```markdown
    naver.com/join.signUp?id=&name=&age=
    ```
   * 동일한 요청 발생시 캐시된 데이터 사용
   * 데이터 길이에 제한이 있음
<br /><br />

- **POST** : 요청 데이터 처리시 사용 <br />
  * 데이터를 Body에 담아 보냄 -> 길이의 제한 없이 전송 가능(대용량 가능)
  * Body를 통해 서버로 요청 데이터를 전달 -> 서버는 Body를 통해 들어온 데이터를 처리하여 응답
  * 신규 리소스 등록, 프로세스 처리에 사용(단순 데이터 생성, 값 변경 => 프로세스 상태 변경)
  * url에 드러나진 않지만 개발자 툴에서 요청 내용 확인 가능 => 암호화 필요
  * 회원가입 & 로그인에 흔히 사용
  * POST의 결과로 새로운 리소스가 생성되지 않을 수 있음
<br /><br />

- **PUT** : 새로운 리소스 생성 & 기존 리소스 수정 (전체 리소스 수정 요청) <br />
    * 클라이언트가 리소스의 위치를 알고 url을 지정
    * 기존에 해당 리소스가 없으면 생섬함

    ```java
    
    /*  1. 기존 데이터
        url: /users    */ 
        {
            "name":"kimJun"
            "address":"daejeon"
        }

    /* 1. 요청받은 body값으로 덮어씌워진(생성된) 데이터 값
        url: /users/info                         */ 
        {
            "name":"IU"
            "address":"Seoul"
        }
    
    ```

    * HTTP 요청의 본문(body)에 포함된 데이터를 사용하므로 꼭 JSON 형태는 아님
    * 일부만 변경하려고 하면 요청 받은 값만 남게 됨

<br />

- **PATCH** : 일부 리소스 수정 요청  <br />
    * 클라이언트가 리소스의 위치를 알고 url을 지정 
    * PATCH를 지원하지 않는 서버가 있을 경우에는 POST사용
  
  ```java
  
    /* address 값만 변경
       url: /users/info                         */ 
        {
            "address":"JeJu"
        }

    // 변경된 결과 값
        {
            "name":"IU"
            "address":"JeJu"
        }
  
    ```

    * 리소스 일부만 변경할 경우에 사용
    * **리소스 전체 변경시 :** <br /> 전체 리소스를 제공 => 요청 및 응답에 불필요한 데이터 전송 가능성 ↑ <br/> 서버는 전체 리소스를 재구성 => 더 많은 처리 과정이 필요

<br />

- **DELETE** : 요청받은 리소스 삭제 <br />
    * 클라이언트가 리소스의 위치를 알고 url을 지정
    * 삭제 요청 -> 서버에서 해당 리소스 존재여부 & 삭제 권한 확인 후 삭제 작업 수행
    * 삭제 성공시 서버는 200번대의 성공 상태 코드 응답
  
<br />

- HEAD : GET방식과 동일하나 메시지 부분(Body)를 제외하고, 상태 줄과 헤더만 반환(페이지의 실제 내용은 불필요하며, 리소스 상태 확인시 사용)

- OPTIONS : 서버가 지원하는 HTTP 메소드를 확인하기 위한 요청
     * 특정 리소스에 대해 어떤 메서드를 허용하는지 확인함
     * CORS(Cross-Origin Resource Sharing) 정책으로 인해 브라우저가 알아서 OPTIONS 요청을 보내고 허용된 HTTP 메소드를 확인함
  
  ```java

    * 보내는 요청
    OPTIONS /example/resource HTTP/1.1
    Host: example.com


    * 받은 응답(허용되는 메소드 목록)
    HTTP/1.1 200 OK
    Allow: GET, POST, PUT, DELETE

  ```

- TRACE : 클라이언트가 보낸 요청을 서버에게 되묻는 요청
    * 웹 서버와 클라이언트 간의 통신 문제를 진단
    * 주로 디버깅 목적으로 사용
    * 서버는 보낸 요청의 헤더와 본문을 그대로 응답하기 때문에 내용 확인 가능 => 요청이 서버에 어떻게 도달하고 처리되는지 확인 가능
    * 중요한 정보 노출 가능성 ↑
    * 보안상의 문제 발생(보안 취약점으로 간주)
    * 실제 운영 환경에서는 대부분 비활성화되어 있음

- CONNECT : 프록시 서버와 클라이언트 간의 통신을 보호하기 위해 사용(터널을 설정하기 위해)
    * CONNECT요청을 보내면 프록시 서버는 해당 호스트와 포트로의 TCP 연결을 설정하고 클라이언트와 웹 서버 간에 터널을 형성해 줌
    * 형성된 터널을 통해 클라이언트는 웹 서버와 안전한 연결을 설정하고, 프록시 서버를 통해 데이터를 안전하게 전송할 수 있게 됨
    * 일반적인 웹 요청과는 다소 다른 용도로 사용되고 있음

<br /><br /><br />


<hr /><br />
<!--
 ** CORS (Cross-Origin Resource Sharing) : 브라우저에서 실행되는 클라이언트 측에서 발생하는 보안 정책으로, 스크립트에서 한 출처(origin)의 리소스가 다른 출처의 리소스와 상호 작용하는 것을 제한함
 <br />
 ** 지정된 url의 주소 값이 동일 할 경우 = " /user/{id} " or " /user/** "

```java
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    @RequestMapping(value = "/user/{id}", method = RequestMethod.GET)
    public ModelAndView getUser(@PathVariable Long id) {
        User user = userService.getUserById(id);
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("user", user);
        modelAndView.setViewName("userDetailPage"); // 이동할 JSP 페이지 설정
        return modelAndView;
    }

    @RequestMapping(value = "/user/{id}", method = RequestMethod.DELETE)
    public String deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return "redirect:/user/list"; // 삭제 후 이동할 URL 설정
    }

    // 다른 컨트롤러 메소드들도 동일한 방식으로 JSP 페이지 설정
}

``` -->
