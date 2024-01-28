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


[^1]: 환경설정 파일