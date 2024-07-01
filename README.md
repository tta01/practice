## HTTP(Hyper Text Transfer Protocol) Method

클라이언트와 서버 사이의 데이터를 주고 받는 방식으로, 사용자가 서버에 요청(request)을 하면 서버는 이에 맞는 응답(response)을 보내준다.
HTTP 메서드는 GET방식은 조회, POST방식은 데이터 생성이나 수정 등 이런 식으로 요청의 목적을 명확하게 표현하는 것에 중점을 두고 있다.

* Google 'Accelerator' 사건 : 'Accelerator'는 해당 페이지에 있는 URL의 내용을 미리 가져오는 것으로 웹페이지의 전환을 빠르게 해주기 위해 개발되었는데,
                              사이트 개발자들이 GET/POST의 개념적 인식없이 사용한 탓에 엉뚱한 데이터가 삭제되는 등의 문제가 발생한 사건


### 주요 메서드 및 관련 메서드
```markdown
GET, POST, PUT, PATCH, DELETE / HEAD, OPTIONS, CONNECT, TRACE
```

---

### 1. 주요 메서드

- **GET**
  - 서버에 단순히 **작업을 요청(조회, 검색)** 할 때 사용되는데, 서버에 있는 리소스에 영향을 주지 않게 즉, **값이나 내용, 상태 등을 바꾸지 않는 경우**에 사용한다.
      * 때문에 **데이터를 읽거나**, **검색**할 때 주 사용되며, 수정할 때는 사용하지 않는다.
      * 데이터 변형의 위험이 없이 사용 가능하기 때문에 **안전하다**고 할 수 있다. 
      
  - GET방식은 요청 전송시 **url에 QueryString을 통해 전송**한다. QueryString은 url의 끝에 ' ? + 이름,값 ' 으로 구성된 요청 파라미터를 의미하는데, 요청 파라미터가 **여러개일 경우 & 로 연결**한다.

  - GET방식은 주소창에 요청 파라미터를 보내고, 응답받기 때문에 **데이터 길이에 제한**이 있다.
      * 데이터 길이가 너무 길어지면 **'Request URI Too Large(414 에러)'** 가 발생한다.
      * 길이의 기준은 브라우저마다 다르지만 왠만해서 GET방식을 사용할 정도라면 넘어갈 일이 없다고 한다.

  - 동일한 요청이 발생하면 이전에 보냈던 **응답을 저장**해둔 브라우저에서 꺼내어 바로 응답해준다. 이를 **캐싱**이라고 한다.
      * 캐싱 덕분에 POST방식보다 GET방식의 속도가 더 빠르다.
      * 캐시를 삭제하면 요청시 서버가 데이터를 다시 가져와서 브라우저에 저장하게 된다.
      * 캐싱때문에 브라우저 히스토리에 응답내역이 남아버리기 때문에 보안에 취약하다.

```markdown
* 요청
naver.com/join.signUp?name=&age=
=> ?부터 QueryString이며 name, age 요청 파라미터가 2가지이기 때문에 &로 연결해줌

* 응답 
naver.com/join.signUp?name=태경&age=20
```

<br /> <br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2Fb95cadbe-757e-4c9d-b287-7fef984addda%2Fimage.png" width="55%" height="45%"/>

<br /><br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2F1ae3d175-c2fc-43f6-875f-8db56de7fde5%2Fimage.png" width="55%" height="45%"/>

<br /><br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2Fe78d9d52-1d49-47ac-9ad5-587ad9dd67ce%2Fimage.png" width="55%" height="45%"/>

<br /> <hr /> <br />


- **POST**
  - 주로 서버의 **리소스를 새로 생성**하거나, **업데이트**할 때 사용한다.
    * 회원가입(계정 생성), 글 작성(글 생성), 프로필 정보 변경(정보 업데이트)

  - 데이터를 URL의 **Body를 통해 서버로 요청 데이터를 전달**해주는데, URL에 데이터 값이 표시 되지 않기 때문에 **길이의 제한 없이** 데이터를 전송할 수 있다.

  - 전송한 데이터가 URL에 드러나진 않지만, 개발자 툴에서 요청 내용 확인 가능함으로 **민감한 내용은 암호화**하여 보내주는 것이 좋다.
    * 즉, GET방식 보다 보안이 좀 더 나을 수 있지만, 안전하다고 할 수는 없다.

  - POST의 결과 값으로 새로운 리소스가 생성되지 않을 수도 있는데, 데이터 업데이트(상태 변경), 트랜잭션 처리 등의 경우 발생한다.
    * 프로필 정보 수정 : 수정되는 내용이 기존 내용에 덮어씌워짐
    * 트랜잭션 처리 : 여러가지 작업이 순서대로 수행되며 전부 다 정상처리 되는 경우를 말함
    
  - POST로 조회시 기술적으로 **캐싱은 가능하지만**, 캐싱하기에 어려운 문제가 있기 때문에 **권장하지 않는다.**
    * POST방식은 서버 상태를 변경하는 작업이 이루어지는데 **캐싱이 되어 버리면 이 요청에서 실제 변경이 발생되지 않거나 동일한 변경이 반복적으로 이루어질 수 있다.**
    * 때문에 POST방식은 기본적으로 캐싱되지 않고, **매번 서버에 직접 요청이 전달**된다.


<br /> <br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2F7e9d26c9-c3ac-4f22-8d0a-82de9de92c20%2Fimage.png" width="55%" height="45%"/>

<br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2Fddb5d0c9-e6c5-41a0-b315-5d0d6310879b%2Fimage.png" width="55%" height="45%"/>

<br />

<img src="https://velog.velcdn.com/images%2Fwoply%2Fpost%2Fe335a4e7-17bc-4830-a662-13a23d1f9738%2Fimage.png" width="55%" height="45%"/>

<br /> <hr /> <br />


- **PUT** 
  - **새로운 리소스를 생성**하거나, 기존의 리소스를 수정할 때 사용한다.
    * 일부를 수정할 때는 사용하지 않으며, **전체 리소스를 수정**할 때 사용한다.
    * 만약 일부 리소스만을 변경하려고 할 경우에는 전체 내용 중에서 요청 받아 응답하여준 결과 값만이 남게 된다.

  - PUT은 클라이언트가 **해당 리소스의 위치가 어디 담겨있는지 알고 URL뒤에 지정**하여 사용한다.
    * 혹시라도 기존에 해당 리소스가 없을 경우에는 새로 생성하여 넣어준다.

  - HTTP 요청의 **본문(body)에 포함된 데이터를 사용**하기 때문에 **여러 형태로 전달**되어 진다.
    * JSON, XML, text/plain 등 다양한 형식으로 사용 가능하다.

    ```java
      * 기존 데이터
      url: /users    
      {
          "name":"TaeGyoung"
          "address":"daejeon"
      }

      * 수정할 데이터
      url: /users/info 
      {
          "name":"IU"
          "address":"Seoul"
      }  

      * 요청받은 body값으로 덮어씌워져서(생성되어) 수정된 데이터 값                  
      {
          "name":"IU"
          "address":"Seoul"
      }
    ```
 
  <br />

- **PATCH** 
  - **일부 리소스를 수정**할 때 사용한다.
    * 전체 리소스를 수정하게 되면 요청 및 응답에 **불 필요한 데이터를 전송**할 가능성이 높고, **서버는 전체 리소스를 재구성**해야 함으로 **더 많은 처리 과정이 필요**하게 되기 때문에 일부 리소스만 변경 하는 것이다.

  - 클라이언트가 **해당 리소스의 위치가 어디 담겨있는지 알고 URL뒤에 지정**하여 사용한다. 

  - PATCH를 지원하지 않는 서버가 있는 경우도 있기 때문에 권장하지 않고, 수정시에는 주로 POST를 쓴다.
  
  ```java
    * 기존 데이터
    url: /users
    {
        "name":"IU"
        "address":"Seoul"
    }

    * address 값만 변경
    url: /users/info 
    {
        "address":"JeJu"
    }

    * 변경된 결과 값
        {
            "name":"IU"
            "address":"JeJu"
        }
  ```

<br />

- **DELETE** 
  - 요청 받은 리소스를 삭제할 때 사용한다.
    * 클라이언트가 **해당 리소스의 위치가 어디 담겨있는지 알고 URL뒤에 지정**하여 사용

  - ① 삭제 요청을 하게 되면 ② 서버에서 해당 리소스 존재여부를 확인하고 ③ 삭제 권한 확인 후 삭제 작업 수행 함
    * 삭제 성공시 서버는 200번대의 성공 상태 코드 응답


---

### 2. 기타메서드

- HEAD : GET방식과 비슷하지만, 실제 문서를 요청하는 것이 아니라, 문서의 정보를 요청하는 것이다. 응답 메세지에는 본문(body)없이 HTTP 헤더만 보낸다.

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


<br /> <hr /> <br />
 
### 3. 멱등성
 
- **멱등성** 
    * 연산을 여러 번 적용해도 결과가 달라지지 않는 성질을 의미한다.
    * 즉, 같은 요청을 계속 보내도 결과 값은 변하지 않는 것을 말하며, GET, PUT, DELETE 가 여기에 속한다.

- **멱등성의 기준**
    * '상태 코드'가 아닌 **'서버에 미치는 영향이 동일한가?'** 를 기준으로 본다. 즉, **결과 값이 같게 출력되는지 확인**하면 되는 것이다.

```text
* 예시
- GET의 경우는 대부분 단순히 값을 읽어오고 있는 메서드이기 때문에 여러번 조회하여도 결과 값이 달라지는 경우는 없다.
- PUT은 같은 리소스로 덮어씌워져서 수정되기 때문에 여러번 호출해도 결과 값이 달라지지 않는다.
- DELETE도 여러 번 호출해도 삭제된 리소스의 값은 변하지 않는다.
    * 처음 삭제시 성공 응답(200)을 받지만, 계속해서 요청을 보내게 되면 에러 응답(404)을 받는다.
    하지만 멱등성의 기준은 '상태 코드'가 아닌 '서버에 미치는 영향이 동일한가?' 이기 때문에 멱등하다고 할 수 있는 것이다. (= 결과 값에 영향이 없음)
```

- **외부요인의 영향**
    * 중간에 외부요인이 끼어들어 리소스를 변경하게 되면, 요청의 결과가 달라질 수 있기 때문에 멱등성이 깨지는 상황이다. 즉, 외부요인에 의해 리소스가 변경되었기 때문에 멱등하지 않아진다.
    * 멱등성을 보장하기 위해서는 외부 요인에 의한 영향을 최소화해아 한다.

<br />

### 4. 안정성 및 캐싱

* 안정성 : 한 번을 호출하든 여러 번을 호출하든 리소스에 **수정이 발생하지 않는 속성**을 의미한다.
```markdown
-  안전한 메소드 : (리소스를 변경하지 않으므로)
  * GET 메소드

- 안전하지 않는 메소드 : (리소스를 변경시키므로)
  * POST, PUT, PATCH, DELETE 메소드 

** 즉, 안전은 리소스가 변하는지 변하지 않는지 파악할 뿐 다른 사항은 고려하지 않음
```

<br />

* 캐 싱 : 요청에 대한 응답을 서버로 부터 1회 받고, 브라우저에 저장되어 있는 것을 의미하며, 추후에 같은 요청을 하였을 때 브라우저에서 요청에 대한 응답을 꺼내어 보여주는 것이다.

<br />

**<정리 >**
메소드명 | 안전성 | 멱등성 | 캐시가능
:--- | :---: | :---: | :---: |
**GET** | <span style="color:red">O<span/> | <span style="color:red">O<span/> | <span style="color:red">O<span/>
**POST** | X | X | X
**PUT** | X | <span style="color:red">O<span/> | X  
**DELETE** | X | <span style="color:red">O<span/> | X 



<!-- 
 ** CORS (Cross-Origin Resource Sharing) : 브라우저에서 실행되는 클라이언트 측에서 발생하는 보안 정책으로, 스크립트에서 한 출처(origin)의 리소스가 다른 출처의 리소스와 상호 작용하는 것을 제한함
 ** 지정된 url의 주소 값이 동일 할 경우 = " /user/{id} " or " /user/** "
 ** URL(Uniform Resource Locator)은 Resource의 정확한 위치 정보(파일의 위치)를 나타냄으로 URL을 통해 Resource가 어디에 있는지 어떻게 접근할 수 있는지 알 수 있다. 

 - **POST와 PUT의 차이점**
  - POST는 새로운 데이터를 생성해 낼 수 있지만, PUT은 사용자가 데이터를 지정하여 해당 리소스를 수정하는 것
  - POST는 요청시마다 데이터를 생성함 / PUT은 같은 요청을 반복해도 같은 결과 값을 얻음 <br />
    * 그렇다고 해서 PUT이 캐싱이 되는건 아님! 요청된 자원을? 상태를? 실시간으로 업데이트 해야 하기 때문에!!

 리소스의 위치를 옮기면 해당 URL을 더 이상 사용할 수 없게 됨
```
원래의 주소값에서  => notion.so/tg0100/10
이 주소로 변경할 경우 => notion.so/tg0100/1234
원래의 주소 값인 'notion.so/tg0100/10' 로 접속했을 때 찾을 수 없는 페이지로 뜨게 됨
```

** **URI와 URL 구분하는 방법** <br/>
URI 는 통합 자원 식별자로 주소에 식별자가 있으면 URI <br />
URL은 리소스 주소를 나타내므로 리소스 위치까지만 나타내면 URL

``` 
https://hstory0208.tistory.com/category

hstory0208.tistory.com 에서 category 라는 경로를 나타냅니다.
category는 리소스의 실제 위치이므로 이 주소는 URL 입니다.
 

https://hstory0208.tistory.com/category/12

hstory0208.tistory.com 에서 category 라는 자원의 경로를 나타내는 부분까진 URL 이지만
/12 는 식별자 이므로 https://hstory0208.tistory.com/category URL을 포함한 URI라고 할 수 있습니다.
 

https://hstory0208.tistory.com/category?page=12

위와 마찬가지로 https://hstory0208.tistory.com/category 까지는 자원의 실제 위치를 나타내기 때문에 URL이고, 뒤의 query ( ?page=12 ) 가 붙었으므로 https://hstory0208.tistory.com/category URL을 포함한 URI 입니다.
```

<br />

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
