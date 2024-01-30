### 서블릿 구현과 실행

실제 애플리케이션의 루트 디렉터리는 "WebContent" 이다.

-> url의 /에서 접근

웹 애플리케이션 루트 디렉터리 바로 하위에 WEB-INF 디렉터리와 WEB-INF[^1] 디렉터리에는 web.xml 파일이 있어야 한다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/cc8f076f-d13c-464a-b1eb-603122a27306)

***

Java SE 프로그램은 개발자가 main() 메소드 안에 구현한 순서대로 실행된다. 즉, 프로그램이 실행되는 순서를 개발자가 제어한다.

그러나 Java EE 기반 프로그램은 실행의 흐름을 컨테이너가 제어한다.

이처럼 개발자가 아닌 제 3자가 프로그램의 실행 흐름을 제어하는 것을 IoC(Inversion of Control), "제어의 역전"이라고 한다.

```java
package com.edu.test;

import java.io.IOException;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;

public class FirstServlet extends HttpServlet{
	@Override
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init() 실행됨!");
	}
	@Override
	public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
		System.out.println("service() 실행됨!");
	}
}
```

***

콜백메소드(callback method): 어떤 객체에서 어떤 상황이 발생하면 컨테이너가 자동으로 호출하여 실행하는 메소드

> init(), service(), destroy() 등

이러한 콜백메소드들이 서블릿을 실행한다.

***

url: http://localhost:8080/edu/portalSite

`@WebServlet`: 클라이언트가 서블릿을 접근하기 위한 경로 지정

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

// 서블릿 실행을 위한 uri를 "\potalSite"로 변경
@WebServlet("/portalSite")
public class SendRedirectTestServlet extends HttpServlet {

	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

		String param = "naver";
		if (param.equals("naver")) {
			resp.sendRedirect("http://www.naver.com");
		} else if (param.equals("daum")) {
			resp.sendRedirect("http://www.daum.net");
		} else if (param.equals("zum")) {
			resp.sendRedirect("http://zum.com");
		} else if (param.equals("google")) {
			resp.sendRedirect("http://www.google.com");
		}
	}
}
```

***

### 요청정보와 응답정보

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/3df6d176-e5d9-45c3-b0e1-4f0533d710fd)

1. 클라이언트로부터 처리 요청받음: 요청받은 페이지가 서블릿이면 서블릿 컨테이너에 처리를 넘기고, 서블릿 컨테이너는 요청받은 서블릿을 WEB-INF/classes나 WEB-INF/lib에서 찾아서 실행 준비를 한다.
2. 최초의 요청 여부 판단: 서블릿 컨테이너는 현재 실행할 서블릿이 최초의 요청인지 판단하고 실행할 서블릿 객체가 메모리에 없으면 최초 요청이고, 이미 있으면 최초의 요청이 아닌 것으로 판단한다.
3. 서블릿 객체 생성: 서블릿 컨테이너는 요청받은 서블릿이 최초의 요청이라면 해당 서블릿을 메모리에 로딩하고 객체를 생성한다. 일반 자바 객체는 new명령문으로 여러 개의 객체를 언제든지 직접 생성할 수 있지만, 서블릿은 최초 요청이 들어왔을 때 한 번만 객체를 생성하고 이때 생성된 객체를 계속 사용한다.
4. init(ServletConfig) 메소드 실행: 주로 객체의 초기화 작업이 구현되어 있다.
5. 서블릿 컨테이너는 HttpServletRequest와 HttpServletResponse 객체를 생성한다.
6. service(HttpServletRequest, HttpServletResponse) 메소드 실행: 실행하는 서블릿의 요청 순서에 상관없이 클라이언트의 요청이 있을 때마다 실행된다. 따라서 service() 메소드에서는 실제 서블릿에서 처리해야하는 내용이 구현되어 있다. 앞서 생성한 두 객체의 주소를 인자로 넘기고. service() 메소드에서는 인자로 박은 두 객체를 사용하여 프로그램을 구현한다.
7. service() 메소드가 완료되면 클라이언트에 응답을 보내고 서버에서 실행되는 프로그램은 완료된다. 이때, requset, response객체는 소멸한다.

***

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.util.*;

@WebServlet("/headerInfo")
public class HeaderInfoServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html;charset=EUC-KR"); // 문서타입과 문자셋 설정, 없을 경우 한글 깨짐
		PrintWriter out = res.getWriter(); // out이라는 출력스트림을 통해 화면에 노출
		out.print("<html>");
		out.print("<head><title>Request 정보 출력 Servlet</title></head>");
		out.print("<body>");
		out.print("<h3>요청 헤더 정보</h3>");
		Enumeration<String> em = req.getHeaderNames();
		while (em.hasMoreElements()) {
			String s = em.nextElement();
			out.println(s + " : " + req.getHeader(s) + "<br/>");
		}
		out.print("</body></html>");
		out.close();
	}
}
```

`Enumeration<String> em = req.getHeaderNames();`
req.getHeaderNames()는 요청정보의 헤더 안에 있는 정보 중 헤더 이름들만 모아 Enumeration 객체에 담아서 반환하고, 반환된 값의 시작주소를 em 변수에 저장합니다. Enumeration 객체도 java. util 패키지에 있으며, 배열처럼 데이터 그룹으로 구성된 Colletion 객체입니다. 제네릭(Generics)을 이용 하여 Enumeration String) 객체 안에 저장되는 데이터의 타입을 String으로 선언하고 있습니다.
제네릭은 Collection 객체에 담기는 데이터의 타입을 Collection 객체 생성 시 미리 선언하는 기능입니다. ArrayList<String> List = new ArrayList<String>(): 이렇게 선언하면 List 안에는 String 타입의 데이터만 저장하겠다는 의미여서 String이 아닌 데이터를 저장하면 오류가 발생합니다. 또한, 추출할 때는 자동으로 String 타입으로 변환됩니다.
Enumertion 객제가 Set. List, Map 계열의 Collection 객제와 다른 점은 그룹의 데이터에 접근할 때 인덱스나 키가 아닌 커서(cursor)라는 개념으로 접근한다는 사실입니다. rect.setHendervame() 메소 드가 헤더 정보의 이름들을 Scing 타입으로 Enumtration 제체에 담아 반원하면 Ehumeration 객지 의 첫 번째 요소 앞에 커서가 위치합니다.

`while (em.hasMoreElements ()) {`
em.hasMoreElements()는 em이 가리키는 Enumeration 객체의 커서 다음에 데이터가 있는지 없는 지를 판단하여 있으면 true, 없으면 false를 반환합니다. 커서가 마지막 요소에 있을 때 비로소 false를 반환하고 while 반복문을 빠져나옵니다.
`String s = em.nextElement ();`
em.nextElement()는 em의 커서 다음에 있는 요소를 반환하고 커서를 다음 요소로 이동시킵니다. em 변수를 선언할 때 Enumeration String>으로 선언했으므로 반환하는 값은 String 타입이며, 반환값을 String s 변수에 저장합니다.

`out.printIn(s + " : " + req. getHeader(s) + "br/)");`
s 변수에는 요청정보의 헤더 이름이 들어있습니다. 헤더 이름을 gettleader( ) 메소드의 인자로 지정하 면 해당 이름을 찾아 값을 반환합니다. 즉, getHeader()는 헤더의 값을 추출할 때 사용하는 메소드입니다.

>  Enumeration과 Iterator는 그룹안에 있는 요소에 접근할 때 인덱스나 키로 접근하는 것이 아니고, 커서의 개념으로 접근한다는 점이다.

***

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

// web.xml을 통해 접근
//@WebServlet("/addInfo")
public class AdditionalInfoServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html;charset=EUC-KR");
		PrintWriter out = res.getWriter();
		out.print("<html>");
		out.print("<head><title>Request 정보 출력 Servlet</title></head>");
		out.print("<body>");
		out.print("<h3>추가적인 요청 정보</h3>");
		out.print("Request Method : " + req.getMethod() + "<br/>");
		out.print("Path Info : " + req.getPathInfo() + "<br/>");
		out.print("Path Translated : " + req.getPathTranslated() + "<br/>");
		out.print("Query String : " + req.getQueryString() + "<br/>");
		out.print("Content Length : " + req.getContentLength() + "<br/>");
		out.print("Content Type : " + req.getContentType() + "<br/>");
		out.print("</body></html>");
		out.close();
	}
}
```

```xml
	<servlet>
		<servlet-name>addInfo</servlet-name> <!-- 3 -->
		<servlet-class>com.edu.test.AdditionalInfoServlet</servlet-class> <!-- 4 -->
	</servlet>
	<servlet-mapping>
		<servlet-name>addInfo</servlet-name>  <!-- 2 -->
		<url-pattern>/addInfo/*</url-pattern> <!-- 1 -->
	</servlet-mapping>
```

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/bab56f88-3680-4d9e-9d9d-6bdf7b983363)

①에서 `<url-pattern>/addInfo</urI-pattern>`으로 지정했으므로 클라이언트로부터 `http://localhost:8080/edu/addInfo`로 URL 요청이 들어오면 ②에 지정된 이름과 같은 이름을 `<servlet>`태그의 `<servlet-name>`에서 찾습니다(③).  그런 다음 `<servlet-class>`에 지정된 서블릿을 실행합니다. 즉, ④에 지정된 `com.edu. test.AdditionallnfoServlet`의 `service( ) `메소드가 실행됩니다. 클라이언트로부터 서비스 요청이 들어와 해당 서블릿이 실행되기까지 매핑된 정보를 찾아가는 순서를 보면 "클라이언트 요청 →① → ② → ③ → ④" 순입니다.
그런데 클라이언트 요청 URL과 매핑되는 `<url-pattern>`을 지정할 때 다음 예처럼 `*` 기호를 사용할 수 있습니다.

***

### 질의 문자열(query string)

: 클라이언트가 웹서버에 서비스를 요청할 때 입력한 데이터들

- name=value 형식으로 전달된다.
- 영문자,숫자,일부 특수문자는 그대로 전달되고, 나머지 문자는 %기호와 함께 16진수로 바뀌어 전달된다.
- 공백문자는 + 기호로 변경되어 전달된다.

#### GET 방식

- 질의 문자열을 요청정보 헤더의 URI에 포함흐므로 서버로 전달되는 값이 브라우저 주소 줄에 모두 노출
- 데이터의 길이가 255바이트 미만
- 질의 문자열이 URI에 포함되어 전달되므로 인코딩/디코딩하는 추가 작업이 필요 없어서 처리속도 면에서 빠름

> GET 방식으로 전달된 질의 문자열들의 인코딩 처리는 server.xml의 <Connector> 태그에 URIEncoding 속성을 UTF-8 속성으로 처리한다.

#### POST 방식

- 문자열의 길이에 제한이 없음
- 전달하는 측은 인코딩, 받는 측은 디코딩 작업이 필요함

```java
req.setCharacterEncoding("UTF-8"); // POST 방식 한글 인코딩 처리
```

***

- EUC-KR: EUC-KR은 한글과 영문만 지원하는 문자코드로서 한 글자를 표현할 때 2Byte를 사용합니다. 단점은 한글 영문 을 제외한 다른 나라 언어는 지원하지 않으므로 다국어 지원이 안 된다는 것이죠, 따라서 국내 사용자만을 대상으로 하는 서 비스에 적합합니다. 장점은 3Byte로 표현되는 UTF-8보다 2Byte로 표현되기 때문에 용량을 적게 차지한다는 것입니다.
- UTF-8: 한글, 영문은 물론 일본어. 중국어 등 전 세계 모든 언어를 표현할 수 있습니다. 따라서 다국어 서비스에 적합합니다.
  그런데 UTF-8의 경우 영어는 1Byte. 한글은 보통 3byte로 표현하기 때문에 EUC-KP보다 용량을 많이 차지한다는 특징이 있습니다. UTF-8 방식은 구글. 페이스북, 트위터 등 세계 주요 웹사이트 1만 개 중 51% 정도 사용하는 등 사용도가 높으며, 여러 가지 웹 관련 최신 기술들이 UTF-8 방식을 기본적으로 지원하고 있습니다.

***

```xml
<!-- web.xml -->
<servlet>
		<servlet-name>initParam</servlet-name>
		<servlet-class>com.edu.test.InitParamServlet</servlet-class>
		<init-param>
			<param-name>id</param-name>
			<param-value>guest</param-value>
		</init-param>
		<init-param>
			<param-name>password</param-name>
			<param-value>1004</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
```

`web.xml`에 매핑한 id와 password의 값을 `init()` 메서드를 오버라이딩해서 사용이 가능하다.

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class InitParamServlet extends HttpServlet{

	String id, password;
	
	@Override
	public void init(ServletConfig config) throws ServletException {
		id = config.getInitParameter("id");
		password = config.getInitParameter("password");
	}
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html;charset=UTF-8");
		PrintWriter out = res.getWriter();
		out.print("<h2>서블릿 초기 추출 변수 </h2>" );
		out.print("<h3>ID : "+id+"</h3>");
		out.print("<h3>PASSWORD : "+password+"</h3>");
		out.close();
	}
}
```

***

### 서블릿 메모리![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/7b82d5a9-8090-4cb7-a4a4-8966ef63d808)

* 멤버변수는 객체 생성 시 힙 메모리에 생성되며 서블릿을 실행하는 클라이언트들이 공통으로 사용
* service() 메서드가 사용하는 지역변수는 스택 메모리에 생성되며, 클라이언트마다 독립적으로 사용

***

### 상태정보 유지기술

HTTP는 비연결형, 무상태로 동작하는 프로토콜이므로 상태정보를 일정 시간 동안 지속해서 유지해주는 기술을 '상태 정보 유지 기술'이리고 한다.

클라이언트 측에 저장하여 유지하는 기술과 서버 측에 저장하여 유지하는 기술이 있다.

> 무상태(Stateless): 클라이언트와 서버 간의 연결을 클라이언트 요청이 있을 때마다 새롭게 연결하는 방식, 서버가 클라이언트에서 응답을 보내는 즉시 끊어짐

- 클라이언트 측에 저장 기술: 웹 브라우저에 저장
  - `javax.xervlet.http.Cookie`
- 서버 측에 저장 기술: 서버의 힙 메모리 영역에 만들어진 객체에 상태정보를 저장
  - `javax.servlet.ServletContext`
  - `javax.servlethttp.HttpSession`
  - `javax.servlet.http.HttpServletRequest`

#### ServletContext

: 웹어플리케이션 단위로 Context를 생성하여 관리, 서블릿 컨테이너와 통신하기 위해서 사용되는 메소드를 지원하는 인터페이스

ServletContext 주솟값을 추출하는 방법

* init() 메소드를 재정의하여 추출하는 방법

```java
	@Override
	public void init(ServletConfig config) throws ServletException{
		ServletContext sc = this.getServletContext(); // getServletContext: ServletContext를 추출하는 메서드	
	}
```

* HttpServlet을 통해 추출하는 방법

```java
ServletContext sc = this.getServletContext();
```

ServletContext 객체는 웹 애플리케이션 단위로 사용되는 객체로, 동일한 웹 애플리케이션 안에 모든 페이지에서 동일한 ServletContext 객체를 사용한다. 그래서  ServletContext 객체를 이용하여 웹 애플리케이션 단위로 정보를 유지함으로써 공유할 수 있다.

***

#### Cookie

: 서버가 클라이언트에 저장하는 정보로서 클라이언트 쪽에 필요한 정보를 저장해놓고 필요할 때 추출하는 것을 지원하는 기술

클라이언트에 저장된 쿠키 정보는 이후 다시 서버에 방문할 때 자동으로 요청정보의 헤더 안에 포함되어 전달됨

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

@WebServlet("/cookie1")
public class CookieTest1Servlet extends HttpServlet {
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();

		Cookie c1 = new Cookie("id", "guest"); //이름이 ID, 값은 guest인 쿠키 생성
		c1.setPath("/"); // 경로 설정
		resp.addCookie(c1);

		Cookie c2 = new Cookie("code", "0001");
		c2.setMaxAge(60 * 60 * 3); // 3시간
		c1.setPath("/");
		resp.addCookie(c2);

		Cookie c3 = new Cookie("subject", "java");
		c3.setMaxAge(60 * 60 * 24 * 10); // 10일
		c1.setPath("/");
		resp.addCookie(c3);

		out.println("쿠키 전송 완료");
		out.close();
	}
}
```

**쿠키 생성 : Cookie(string name, String value)**
쿠키를 생성하려면 javax.servlet.http.Cookie 객체를 생성합니다. Cookie 객체는 두 개의 String을 인자로 인자로 받는 생성자가 선언되어 있는데요. 
첫 번째 인자가 쿠키의 name으로 지정되며, 두 번째 인자가 쿠키의 value로 지정됩니다.

**쿠키 유효 시간 설정 : setMaxAge(int expiry)**
클라이언트로 전송되는 쿠키의 유효 시간을 설정한 때는 Cookie 객체의 setMaxAge() 메소드를 사용합니다. 인자값으로 정수를 지정하며, 이 값은 Cookie의 유효 시간의 초(second)를 의미합니다.
지정된 인자값에 따라 쿠키 동작이 다르게 되는데요. 만약 정숫값을 0으로 지정하면 쿠키 삭제를 의미합니다. 그리고 음수값을 지정하면 쿠키가 클라이언트로 전송된 후 브라우저가 종료되면 쿠키도 자동으로 삭제됩니다. 개발 시 setMaxAge() 메소드로 유효 시간을 지정하지 않은 쿠키도 음수값이 적용되어 브라우저가 전송받은 후 브라우저가 종료되면 쿠키도 함께 삭제됩니다.

**쿠키 경로 설정 : setPath(String uri)**
현재 접속 중인 서버에서 이전에 클라이언트에게 전송한 쿠키가 있으면 기본적으로 요청정보 해더 안에 쿠키가 포함되어 서버 쪽으로 전송됩니다. 서버의 모든 요청에 대하여 쿠키가 서버 쪽으로 전송되는 것이 아니라, 특정 경로의 요청에서만 쿠키를 전송하고자 할 때 setPath() 메소드를 사용하여 경로를 지정할 수 있습니다. setPath() 메소드의 인자값으로 경로를 지정하면, 지정된 경로와 그것의 하위 경로의 요청에 대해서만 클라이언트로부터 쿠키가 전송됩니다.

**쿠키 도메인 설정 : setDomain(String domain)**
쿠키는 기본적으로 전송된 서버에서만 읽어 들여 사용할 수 있습니다. 그런데 어떤 웹 서비스는 하나의 서버에서만 전체 서비스를 하는 것이 아니라, 여러 대의 서버가 연결되어 서비스를 처리합니다. 이러한 웹 서비스에서는 쿠키의 도메인 설정을 통해 하나의 서버에서 클라이언트로 전송된 쿠키를 다른 서버에서 읽어 들여 사용할 수 있습니다. 이때 사용하는 메소드가 setDomain()입니다.
setDomain() 메소드의 인자값으로 서버의 도메인을 지정하는데 `www.edu.com`처럼 지정하면 정확히 일치하는 도메인에서만 쿠키를 읽어 들일 수 있고, `.edu.com`처럼 지정하면 `it. edu.com`이나 `math,edu.com`처럼 `edu.com`이 포함된 모든 도메인 서버에서 쿠키를 읽어 들일 수 있습니다.

**쿠키 전송 : addCookie(Cookie cooke)**
생성된 쿠키를 클라이언트로 보낼 때는 HttpServetResponse 객체의 addCooktel( ) 메소드를 이 용합니다. addCookie() 메소드의 인자값으로 전송할 Cookie 객체를 설정하면 쿠키에 설정된 내용 으로 클라이언트 쪽으로 쿠키가 전송됩니다.

**쿠키 추출 : Cookie[] getCookies( )**
클라이언트로 전송된 쿠키를 서버 쪽에서 읽어 들이려면 HttpServletRequest 객체의 getCookies메소드를 이용합니다.

**쿠키 검색 : String getName()**
HttpServletRequest 객체의 getCookies() 메소드는 서버가 전송한 쿠키를 한꺼번에 읽어 들여 반환하므로 반환된 쿠키 중에서 원하는 쿠키를 찾는 작업을 해야 합니다. 쿠키를 검색할 때는 쿠키의 이 름을 가지고 검색하며, 쿠키의 이름만을 추출할 때는 Cookie 객체의 getName( ) 메소드를 사용합 니다.

**쿠키 값 추출 : String getValue( )**
읽어 들인 쿠키 중에서 원하는 쿠키를 이름으로 검색하여 찾은 다음에는 쿠키의 값을 추출하여 사용 해야 합니다. 쿠키의 값을 추출할 때는 Cookie 객체의 getValue() 메소드를 사용합니다.

***

#### Session

: Http 기반으로 동작하는 클라이언트가 서버에 정보를 요청할 때 생성되는 "상태정보"를 세션이라고 한다.

HttpSession 객체가 생성될 때는 요청을 보내온 클라이언트 정보, 요청 시간 정보 등을 조합한 세션ID가 부여되며, 이 세션 ID는 클라이언트 측에 쿠키 기술로 저장된다.

즉, HttpSession 객체는 서버에 생성되며, 클라이언트에는세션ID가 쿠키 기술로 저장되어 각 클라이언트에 대하여 생성되는 HttpSession 객체를 클라이언트마다 개별적으로 유지 및 관리한다.

**HttpServletRequest의 getSession()**

클라이언트가 가지고 있는 세션ID와 동일한 세션 객체를 찾아서 주솟값을 반환, 만일 세션이 존재하지 않으면 새로운 HttpSession 객체를 생성하여 반환한다.

**HttpServletRequest의 getSession**(boolean create)

클라이언트가 가지고 있는 세션ID와 동일한 세션 객체를 찾아서 주솟값을 반환, 만일 세션이 존재하지 않으면 매개변수 create의 값이 true인지 false인지에 따라 다르게 동작한다.  true이면 getSession메소드와 마찬가지로 새로운 HttpSession 객체를 생성 및 반환하고, false이면 null을 반환한다.

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

@WebServlet("/sessionTest")
public class SessionTestServlet extends HttpServlet {

	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();
		String param = req.getParameter("p");
		String msg = null;
		HttpSession session = null;

		if (param.equals("create")) {
			session = req.getSession();
			if (session.isNew()) {
				msg = "새로운 세션 객체가 생성됨";
			} else {
				msg = "기존의 세션 객체가 리턴됨";
			}
		} else if (param.equals("delete")) { 
			session = req.getSession(false); // 세션객체가 없으면 null
			if (session != null) {
				session.invalidate(); // 세션 객체 삭제
				msg = "세션 객체 삭제 작업 완료";
			} else {
				msg = "삭제할 세션 존재하지 않음";
			}
		} else if (param.equals("add")) {
			session = req.getSession(true);
			session.setAttribute("msg", "메시지입니다");
			msg = "세션 객체에 데이타 등록 완료";
		} else if (param.equals("get")) {
			session = req.getSession(false);
			if (session != null) {
				String str = (String) session.getAttribute("msg");
				msg = str;
			} else {
				msg = "데이타를 추출할 세션 객체 존재하지 않음";
			}
		} else if (param.equals("remove")) { 
			session = req.getSession(false);
			if (session != null) {
				session.removeAttribute("msg");
				msg = "세션 객체의 데이타 삭제 완료";
			} else {
				msg = "데이타를 삭제할 세션 객체 존재하지 않음";
			}
		} else if (param.equals("replace")) { 
			session = req.getSession();
			session.setAttribute("msg", "새로운 메시지입니다");
			msg="세션 객체에 데이타 등록 완료";
		}

		out.print("처리 결과 : " + msg);
		out.close();
	}
}
```

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/7c323dd9-1e00-43c6-b483-babbe8a3ef30)

* [로그인/로그아웃 세션 구현](https://github.com/siwoo1627/java/blob/main/Servlet/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8%EC%95%84%EC%9B%83/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8%EC%95%84%EC%9B%83.md)

| 구분             | 쿠키       | 세션                       |
| ---------------- | ---------- | -------------------------- |
| 저장 위치        | 클라이언트 | 서버                       |
| 저장 데이터 타입 | 텍스트     | 객체                       |
| 저장 데이터 크기 | 제한 있음  | 서버에 수용할 수 있는 만큼 |

***

#### HttpServletRequest

: A페이지와 B페이지가 동시에 실행되거나 A페이지를 통해 B페이지가 실행될 경우 HttpServletRequset 객체가 유지되므로 해당 객체로 정보 공유함

> service() 메소드가 실행되기 전에 자동으로 생성되고, service() 메소드가 종료되면 자동을 소멸하는 객체

HttpServletResponse 요청 재지정[^2]

* sendRedirect(String location)
  * `resp.sendRedirect("http://www.naver.com");`

* encodeRedirectURL(String url)

RequestDispacther 요청 재지정

* forward: HttpServletRequst, HttpServletResponse 객체를 다른 자원에 전달하고 수행 제어를 완전히 넘겨서 다른 자원의 수행 결과를 클라리언트로 응답하도록 하는 기능을 하는 메서드
* include: HttpServletRequst, HttpServletResponse 객체를 다른 자원에 전달하고 수행한 다음, 그 결과를 클라이언트에서 요청한 서블릿 내에 포함하여 클라이언트에 응답하는 기능의 메서드

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

@WebServlet("/dispatcher1")
public class DispatcherTest1Servlet extends HttpServlet {

	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();
		out.print("<h3>Dispatcher Test1의 수행결과</h3>");

		ServletContext sc = this.getServletContext();
		RequestDispatcher rd = sc.getRequestDispatcher("/dispatcher2");
        // rd.forward(req, resp);
		rd.include(req, resp);

		out.close();
	}
}
```

* [책정보](https://github.com/siwoo1627/java/blob/main/Servlet/%EC%B1%85%EC%A0%95%EB%B3%B4/bookInput.html)

***

### Filter

: 클라이언트로부터 서블릿이 요청되어 수행될 때 필터링 기능을 제공하기 위한 기술

> 인증, 로그, 오류 처리, 데이터 압축, 변환, 응답 내용 추가, 한글처리 등

**init(FilterConfig)**

필터 객체가 생성될 때 호출되는 메소드, 웹서버가 시작될 때 한 번만 생성된 다음, 계속 사용되므로 init()메소드는 주로 초기화 기능을 구현한다.

**destroy()**

필터 객체가 삭제될 때 호출되는 메서드

**doFilter(ServletRequest, ServletResponse, FilterChain)**

필터링 설정한 서블릿을 실행할 때마다 호출되는 메소드로서 실제 필터링 기능을 구현하는 메소드

```java
package com.edu.test;

import javax.servlet.*;

public class FlowFilterOne implements Filter {
	public void init(FilterConfig config) {
		System.out.println("init() 호출 ......... one");
	}

	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws java.io.IOException, ServletException {
		System.out.println("doFilter() 호출 전 ........ one");
		chain.doFilter(req, res);
		System.out.println("doFilter() 호출 후 ........ one");
	}

	public void destroy() {
		System.out.println("destroy() 호출 ........ one");
	}
}
```

필터 등록 : `<fiter>` 태그
Filter 인터페이스를 상속받아 필터 객체를 구현한 다음에는 구현된 필터를 서버에 등록해야만 동작한다. 서버에 필터를 등록하는 방법은 weh. xml 파일에 다음과 같은 태그를 사용합니다.

```xml
<!-- 필터 등록 태그 구조 -->

<filter>
	<filter-name>flow1</filter-name>
    <filter-class>com.edu.test.FlowFilterOne</filter-class>
	<filter-name>flow2</filter-name>
    <filter-class>com.edu.test.FlowFilterTwo</filter-class>
	<init-param>
		<param-name>en</param-name>
		<param-value>UTF-8</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>flow1</filter-name>
	<url-pattern>/second</url-pattern>
</filter-mapping>
<filter-mapping>
	<filter-name></filter-name>
    <servlet-name></servlet-name>
</filter-mapping>
```

* `<filter>`: javax servlet.Fiiter 인터페이스를 상속받고 있는 객체를 등록하는 태그입니다. 웹 애플리케이션 서비스가 준비되면서 웹서버가 web.Xml 파일에서 `<fiter>` 태그를 읽으면 해당하는 객체를 찾아가 객체를 생성합니다. 그래서 필터 객체가 생성되는 시점은 웹 애플리케이션 서비스가 시작될 때입니다.
* `<filter-name>`: 등록하는 필터의 논리적인 이름을 지정합니다. 이름은 개발자가 임의로 지정하는 이름으로서 서버에 필터를 등록하는 이름입니다. 이름을 등록한 후에는 클래스 이름이 아니라 등록된 이름으로 사용해야 합니다. `<fiter-name>`은 `<filter>` 태그의 필수 하위 태그입니다.
* `<filter-class>`: 등록하는 필터의 클래스 이름을 지정합니다. 웹서버는 `<filter-class>`에 등록된 클래스를 찾아가 객체를 생성하기 때문에 실제 클래스 이름을 패키지 이름과 함께 정확하게 지정해야 객체가 올바르게 생성됩니다. `<filer-class>`는 `<filter>` 태그의 필수 하위 태그입니다.
* `<init-param>` : web.xml에서 필터 객체에 변수를 전달할 때 사용하는 태그입니다. `<init-param>`은 `<filter>` 태그의 선택 하위 태그입니다.
* `<param-name>` : 필터 객체에 전달하고자하는 변수의 이름을 지정합니다. `<param-name>`은 `<init-param>` 태그의 필수 하위 태그입니다.
* `<param-value>` : 필터 객체에 전달하려는 변수값을 지정합니다. `<param-value>`는 `<init-param>` 태그의 필수 하위 태그입니다.

* `<filter-mapping>` : 앞에서 `<filter>` 태그로 등록된 필터가 어떤 서블릿을 필터랑할 것인지 설정하는 태그입니다.
* `<filter-name>`: 실행할 필터를 지정합니다. 이때 지정하는 값은 필터의 클래스 이름이 아니고,  `<filter>` 태그로 필터를 등록 할 때 `<filter-name>` 태그에 지정한 이름을 사용해야 합니다. 만일 등록되지 않은 필터 이름을 지정한다면 web.xml 오류가 발생하여 웹 애플리케이션 전체가 서비스되지 않습니다.
* `<url-pattern>`: 필터링할 서블릿을 지정합니다. 서블릿을 지정할 때는 클라이언트가 요청하는 URL을 지정합니다. URL을 지정할 때는 전체 주소를 지정하는 것이 아니라. 웹 애플리케이션 이름까지는 생략한 후 다음부터 지정합니다.
  예를 들어. `http://ocalhost:8080/edu/second` 요청에 대하여 실행되는 페이지를 필터링하고자 한다면, 앞에는 생략하고  second만 지정하면 됩니다. 어차피 현재 web.xml은 `/edu` 웹 애플리케이션에 대한 설정이므로, 별도로 지정하지 않아도 고정되어 있기 때문입니다. 그리고 url로 필터링할 페이지를 지정하기 때문에 꼭 서블릿만 필터링할 수 있는 것은 아닙니다.
* `<servlet-name>`: 필터링할 서블릿을 지정할 때 `<url-pattern>` 태그로 클라이언트가 실행을 요청하는 url로 지정할 수 있지만, `<servlet-name>` 태그를 사용할 수도 있습니다. `<servlet-name>`의 값은 `<servlet>`의 `<servlet-name>` 태그에서 지정된 값만을 사용할 수 있습니다. 만일 등록되지 않은 값이 `<servlet-name>` 태그값으로 사용되면 웹 애플리케이션은 올바르게 서비스되지 않습니다.

```java
chain.doFilter(req, res);
```

chain 변수는 doFilter() 메소드가 호출될 때 세 번째 인자로 전달되는 FilterChain 객체이다. FilterChain 객체는 web.xml 파일에서 모든 `<filter-mapping>` 태그에 관한 정보를 근거로 해서, 현재 실행중인 메소드 다음에 실행할 메소드를 실행하는 역할을 한다.

> flow1이 실행되고 flow2가 실행됨

더 이상 실행할 filter가 없으면 service() 메소드가 호출된다.

#### 한글 처리

```java
package com.edu.test;

import javax.servlet.*;

public class FlowFilterTwo implements Filter {	
	String charset;
	public void init(FilterConfig config) {
		System.out.println("init() 호출 ......... two");
		charset = config.getInitParameter("en");
	}

	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws java.io.IOException, ServletException {
		req.setCharacterEncoding(charset);
		System.out.println("doFilter() 호출 전 ........ two");
		chain.doFilter(req, res);
		System.out.println("doFilter() 호출 후 ........ two");
	}

	public void destroy() {
		System.out.println("destroy() 호출 ........ two");
	}
}
```

```java
public void init(FilterConfig config) {
```

: 필터 객체에 대한 정보는 web.xml 파일에 `<filter>` 태그로 설정되어 있으며 `<init-param>` 태그 정보 역시 추출 가능하다.

#### @WebFilter 어노테이션

* @WebFilter("/login"): 클라이언트가 요청하는 페이지의 실행 요청 URl을 설정하는 방법
* @WebFilter("/*"): 와일드 카드를 사용하여 설정하는 방법
* @WebFilter(value="/hello"): value 속성으로 지정하는 방법
* @WebFilter(urlPatterns="/hello"): urlPatterns 속성으로 지정하는 방법
* @WebFilter(servletNames="/MyServlet"): 서블릿 이름으로 지정하는 방법
* @WebFilter(servletNames="FirstServlet","SecondServlet"): 값이 여러 개일 때 배열처럼 지정하는 방법

```java
package com.edu.test;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;

@WebFilter(filterName = "timer", urlPatterns = { "/third" })
public class FlowFilterThree implements Filter {
	public void init(FilterConfig config) {}

	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws java.io.IOException, ServletException {
		long startTime = System.currentTimeMillis();
		chain.doFilter(req, res);
		long endTime = System.currentTimeMillis();
		long executeTime = endTime - startTime;
		System.out.println("수행 시간  :" + executeTime + " ms");
	}

	public void destroy() { }
}
```

***

### 리스너(Listener)

: 어떠한 이벤트가 발생하기를 기다리다가 실제 그 이벤트가 발생했을 때 수행되는 메소드를 가지고 있는 자바 객체[^3]

| 구분               | 생성 시점                 | 삭제 시점                 |
| ------------------ | ------------------------- | ------------------------- |
| ServletContext     | 서버 시작 시              | 서버 종료 시              |
| HttpSession        | 클라이언트 접속 시        | 클라이언트 접속 종료 시   |
| HttpServletRequest | 클라이언트 서비스 요청 시 | 클라이언트 서비스 응답 시 |

```xml
<listener>
	<listener-class>com.edu.test.TestRequestListener</listener-class>
</listener>
```

`<listner>`: 이벤트 핸들러를 상속받아 메소드를 재정의한 객체, 즉 리슨너 객체를 등록할 때 사용하는 태그

`<listener-class>`: `<listener>` 태그를 사용할 때 반드시 지정해야 하는 태그로서, 실제 리스너 객체의 클래스 이름을 패키지 이름과 함께 정확하게 지정함

> 웹서버가 시작될 때 web.xml에서 listener 태그를 읽어 들이면서 listener-class에 지정한 클래스를 찾아서 생성하고 서버가 중지될 때 삭제된다.
>
> 웹 애플리케이션이 서비스되고 있는 동안 메모리에 상주하고 있으므로 이벤트가 발생하면 자동으로 메소드가 실행된다.

#### HttpServletRequest

```java
package com.edu.test;

import javax.servlet.*;

public class TestRequestListener implements ServletRequestListener {
	public TestRequestListener() {
		System.out.println("TestRequestListener 객체 생성");
	}

	public void requestInitialized(ServletRequestEvent e) {
		System.out.println("HttpServletRequest 객체 초기화");
	}

	public void requestDestroyed(ServletRequestEvent e) {
		System.out.println("HttpServletRequest 객체 해제");
	}
}
```

#### HttpSession

```java
package com.edu.test;

import javax.servlet.http.*;

public class TestSessionAttributeListener implements HttpSessionAttributeListener {
	public TestSessionAttributeListener() {
		System.out.println("TestSessionAttributeListener 객체 생성");
	}

	public void attributeAdded(HttpSessionBindingEvent e) {
		System.out.println("세션 객체에 속성 추가");
	}

	public void attributeRemoved(HttpSessionBindingEvent e) {
		System.out.println("세션 객체에 추가된 속성 삭제");
	}

	public void attributeReplaced(HttpSessionBindingEvent e) {
		System.out.println("세션 객체에 추가된 속성 대체");
	}
    
	public void sessionCreated(HttpSessionEvent e) {
		System.out.println("세션 객체 생성");
	}

	public void sessionDestroyed(HttpSessionEvent e) {
		System.out.println("세션 객체 해제");
	}
}
```

#### ServletContext[^4]

```java
package com.edu.test;

import javax.servlet.*;
import javax.servlet.annotation.WebListener;

@WebListener
public class TestServletContextListener implements ServletContextListener {
	public TestServletContextListener() {
		System.out.println("TestServletContextListener 객체 생성");
	}

	public void contextInitialized(ServletContextEvent e) {
		System.out.println("ServletContext 객체 초기화");
	}

	public void contextDestroyed(ServletContextEvent e) {
		System.out.println("ServletContext 객체 해제");
	}
}
```

#### @WebListener 어노테이션

@WebListener 추가 시 web.xml에 작업 안해도됨

***

### 오류 처리

#### 실행 코드를 try-catch 블록으로 구성한다

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

@WebServlet("/errorTest2")
public class ErrorTest2Servlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html;charset=UTF-8");
		PrintWriter out = res.getWriter();
		try {
			String getquery = req.getQueryString();
			out.print("Query : " + getquery + "<br>");
			out.print("Query 길이 : " + getquery.length());

		} catch (Exception e) {
			out.print("요청을 처리하는 동안 오류가 발생하였습니다 : " + e);
		}
		out.print("<br>Done!");
		out.close();
	}
}
```

#### 메소드 선언부에 throws 절을 선언한다

throws 다음에 선언된 Exception 객체들은 현재 메서드에서 발생할 오류인데, 메소드 내에서 처리하지 않고 메소드를 호출한 곳에서 처리하라고 선언하는 것이다.

```java
public void b() throws Exception{
	// 오류   
}
```

b() 메소드에서 Exception을 throws하면 b() 메소드를 호출한 곳에서는 오류를 처리해야 한다. 

```java
public void a(){
    try{
        b();
    }catch(Exception e){
        // b에서 발생한 오류 처리
    }   
}
```

#### web.xml에 오류 처리를 설정한다

오류가 발생했을 때 처리할 페이지를 web.xml 파일에 지정하면, 오류가 발생했을 때 프로그램이 강제로 중단되지 않고 web.xml에 설정한 오류 처리 페이지를 실행한다.

* web.xml을 이용해서 오류를 처리하면 대상이 현재 웹 애플리케이션 전체이다.

> try-catch와 web.xml이 동일오류에 대한 처리일 경우 try가 우선시된다.

```xml
<error-page>
	<error-code>405</error-code>
	<location>/errorHandle</location>
</error-page>
<error-page>
	<exception-type>java.lang.NullPointerException</exception-type>
	<location>/errorHandle</location>
</error-page>
```

`<error-page>`: 웹 애플리케이션에서 발생하는 오류를 처리할 때 사용하는 태그로서 처리할 오류와 처리할 페이지가 무엇인지를 값으로 지정한다. 처리할 오류는 error-code나 exception-type 태그에 지정하며, 오류를 처리할 페이지는 location 태그에 지정한다.

`<error-code>`: 처리할 오류를 지정하는 태그로서 오류 코드로 값을 지정한다.

`<exception-type>`: 처리할 오류를 지젖ㅇ하는 또 다른 태그로서 오류가 정의된 객체 이름으로 지정한다. 객체 이름은 패키지 이름과 함께 정확하게 지정해야 한다.

`<location>`: 오류가 발생했을 때 실행할 페이지 경로를 지정한다. `<location>`에 지정한 페이지는 오류가 발생하면 자동으로 실행된다.

```java
package com.edu.test;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;

@WebServlet("/errorHandle")
public class ErrorHandleServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html;charset=UTF-8");
		PrintWriter out = res.getWriter();

		Integer code = (Integer) req.getAttribute("javax.servlet.error.status_code");
		String message = (String) req.getAttribute("javax.servlet.error.message");
		Object type = req.getAttribute("javax.servlet.error.exception_type");
		Throwable exception = (Throwable) req.getAttribute("javax.servlet.error.exception");
		String uri = (String) req.getAttribute("javax.servlet.error.request_uri");

		out.print("<h2>Error Code    : " + code + "</h2>");
		out.print("<h2>Error Message : " + message + "</h2>");
		out.print("<h2>Error Type    : " + type + "</h2>");
		out.print("<h2>Error Object: " + exception + "</h2>");
		out.print("<h2>Error URI     : " + uri + "</h2>");

		out.close();
	}
}
```

***


[^1]: 환경설정 파일
[^2]: 클라이언트가 요청한 페이지가 실행되다가 다른 페이지로 이동하는 것
[^3]: 이벤트 핸들러
[^4]: 가장 많이 사용