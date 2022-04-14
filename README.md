# springMvc2_first
스프링 MVC 2편 - 백엔드 웹 개발 활용 기술 - 타임리프

- [[${data}]]
- <span th:text="${data}">
- Controller -> thymeleaf
  - 이스케이프(escape)
    - <  \&lt;
    - \> \&gt;
  - Unescape
    - th:utext
    - [(${data})]



- springEL

  - Value

    - <span th:text="${user.username}">
    - <span th:text="${user['username']}">
    - <span th:text="${user.getUsername()}">

    - List
      - <span th:text="${users[0].username}">
      - <span th:text="${users[0]['username']}">
      - <span th:text="${users[0].getUsername()}">

  - Map
    - <span th:text="${userMap['userA'].username}">
      - <span th:text="${userMap['userA']['username']}">
      - <span th:text="${userMap['userA'].getUsername()}">



- 기본객체들
  - ${param.paramData}
  - ${#request}
  - ${#response}
  - ${#session.sessionData}
  - ${#servletContext}
  - ${#locale}



- 

  ```java
  model.addAttribute("lovalDataTime", LocalDataTime.now());
  ```

- <span th:text="${temporals.format(localDataTime, "yyyy-mm-dd HH:mm:ss")}"



- URL 링크
  - \<a th:href="@{/hello}">basic url</a>
  - \<a th:href="@{/hello(param1=${param1}, param2=${param2})}">
  - \<a th:href="@{/hello/{param1}/{param2}(param3=${param3})}">



- 타임리프에서 문자 리터널은 항상 '(작은따옴표)로 감싸야함
- 공백없는 문자는 작은 따옴표 생략 가능
  - <span th:text="'hello'">
- 공백있는 문자열은 작은따옴표 붙여야함
  - <span th:text="'hello world'">
  - <span th:text="'hello world' + ${data}">
    - 작은따옴표 >> 리터널로 대체
    - <span th:text="|hello world ${data}|"



- 연산
- <span th:text="3 &gt 1"
- <span th:text="1 &lt 10"
- <span th:text="(10 % 2 == 0)? '짝수':'홀수'"



- th:classappend="large"
- <input type="checkbox" name="active" th:checked="true">
- <input type="checkbox" name="active" th:checked="false">



- ```java
  List<User> list = new ArrayList<>();
  list.add(new User("gbitkim", 1));
  list.add(new User("dbitkim", 2));
  list.add(new User("ebitkim", 3));
  ```

- ```java
  <tr th:each="user : ${users}">
    <td th:text="${user.username}">userName</td>
    <td th:text="${user.age}">age</td>
    <td th:text="${userStat.index}">index</td>
    <td th:text="${user.size}">size</td>
    <td th:text="${user.count}">count</td>
    <td th:text="${user.even}">even</td>
    <td th:text="${user.current}">even</td>
      
      
    <td th:text="미성년자" th:if="${user.age lt 20}"></td>
    <td th:text="미성년자" th:unless="${user.age gt 20}"></td>
  </tr>
      
  <td th:switch="${user.age}">
    <span th:case="10">10살</span>
    <span th:case="20">20살</span>
    <span th:case="*">기타</span>
  </td>
  ```

- <th:block th:each="user : ${users}">

  - 유일한 자체태그





- 자바스크립트 인라인 적용

- ```java
  @GetMapping("/javascript")
  public String javascript(Model model) {
      model.addAttribute("user", new User("UserA", 10));
      addUsers(model);
      return "basic/javascript";
  }
  ```

- ```html
  <!-- 자바스크립트 인라인 사용 전 -->
  <script>
      var username = [[${user.username}]];
      var age = [[${user.age}]];
  
      //자바스크립트 내추럴 템플릿
      var username2 = /*[[${user.username}]]*/ "test username";
      
      //객체
      var user = [[${user}]];
  </script>
  
  <!-- 자바스크립트 인라인 사용 후 -->
  <script th:inline="javascript">
      var username = [[${user.username}]];
      var age = [[${user.age}]];
  
      //자바스크립트 내추럴 템플릿
      var username2 = /*[[${user.username}]]*/ "test username";
  
      //객체를 JSON으로 넣어준다 
      var user = [[${user}]];
  </script>
  ```



위 내용을 소스보기를 하면

```java
<script>
    var username = UserA;
    var age = 10;

    //자바스크립트 내추럴 템플릿
    var username2 = /*UserA*/ "test username";
    
    //객체
    var user = BasicController.User(username=UserA, age=10);
</script>

<!-- 자바스크립트 인라인 사용 후 -->
<script>
    var username = "UserA"; // 쌍따옴표 자동으로 들어가게 해준다
    var age = 10; // 숫자는 자동으로 쌍따옴표 안쓴다

    //자바스크립트 내추럴 템플릿
    var username2 = "UserA";

    //객체를 json으로 변환해준다
    var user = {"username":"UserA","age":10};
</script>
```



- js 인라인 for문 객체 찍기

- ```java
  <script th:inline="javascript">
      [# th:each="user, stat : ${users}"]
      var user[[${stat.count}]] = [[${user}]];
      [/]
  </script>
  ```

- 결과

  - ```html
    <!-- js forEach -->
    <script>
        var user1 = {"username":"userA","age":10};
        var user2 = {"username":"userB","age":20};
        var user3 = {"username":"userC","age":30};
    </scirpt>
    ```
