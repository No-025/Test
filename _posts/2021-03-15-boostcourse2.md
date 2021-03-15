---
title: "boostcourse 풀스택: 서블릿"
toc: tru
toc_label: "서블릿"
categories:
  - Spring
  - Servlet
---
## Servlet

```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	response.setContentType("text/html;charset=UTF-8");
	PrintWriter out = response.getWriter();
	out.print("<h1>Hello Servlet</h1>");
}
```
>첫 서블릿 컴파일 및 실행  
>
![](https://blog.kakaocdn.net/dn/biB7lO/btqZpeS9jTY/RlUItEQDKZ2frPwmULhAB1/img.png)

----------

## Servletd이란?

자바 웹 어플리케이션의 구성요소 중 동적인 처리를 하는 프로그램의 역할
WAS에서 동작하는 java클래스
HttpServlet클래스를 상속받아야 함
HTML은 JSP로 표현, 복잡한 프로그래밍은 서블릿으로 구현

----------

## Servlet 작성방법

1. **3.0이상** 버전
자바 어노테이션(annotation) 사용(@을 앞에 붙인)
> 3.1 버전  
```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	response.setContentType("text/html;charset=UTF-8"); // 응답 컨텐츠 타입 지정
	PrintWriter out = response.getWriter(); // 문자열을 출력할 수 있는 PrintWriter 구함
	out.print("<h1>1-10까지 출력</h1>");
	for(int i = 0; i < 10; i++){
		out.println(i+1 + "<br>");
	}
	out.close();
}
```  
  
  
3. **3.0이하** 버전
web.xml파일에 등록
>2.5버전 web.xml  
>
![](https://blog.kakaocdn.net/dn/QlqJT/btqZrgXb7an/jSZn1imsYjKs4aguZ5QXe0/img.png)

----------

## Serverlet 라이프사이클

![](https://blog.kakaocdn.net/dn/omoCk/btqZqo2iD6I/QZkA8v2Pb4IZkimkcSFhE1/img.png)

> Servlet 라이프사이클 예
> 
**Servlet 객체 생성**: 최초 *한번*
**Init() 호출** -> 최초 *한번*
**service(), doGet(), doPost() 호출** -> 요청시 *매번*
**destroy() 호출** -> 마지막 *한번* (servlet 수정, 서버 재가동 등)

---

## WAS가 웹브라우저로부터 요청 받으면
![](https://blog.kakaocdn.net/dn/kZe9p/btqZDPSPY7r/xC3kbkrwG5anR763lcU82K/img.png)

*(출처: boostcourse)*

1. 요청할 때 가지고 들어온 다양한 정보들을  HttpServletRequest객체를 생성하여 저장
2. 웹 브라우저에 응답을 보낼 정보들을  HttpServletResponse객체를 생성하여 저장
3. 생성된 HttpServletRequest, HttpServletResponse 객체를 서블릿에게 전달

----------

## Header정보 읽어 들이기

```
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");

		Enumeration<String> headerNames = request.getHeaderNames();
		while(headerNames.hasMoreElements()) {
			String headerName = headerNames.nextElement();
			String headerValue = request.getHeader(headerName);
			out.println(headerName + " : " + headerValue + " <br> ");
		}		
		
		out.println("</body>");
		out.println("</html>");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
```

----------

## 파라미터 읽어 들이기

URL주소의 파라미터 정보를 읽어 들여 브라우저 화면에 출력함

```
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");

		String name = request.getParameter("name");
		String age = request.getParameter("age");
		
		out.println("name : " + name + "<br>");
		out.println("age : " +age + "<br>");
		
		out.println("</body>");
		out.println("</html>");
	}
```
>파라미터가 없는 경우
>
![](https://blog.kakaocdn.net/dn/bNKI0X/btqZtzcvjnK/OupdSYw2xzwiFuQRYyykr1/img.png)
>파라미터가 있는 경우

![](https://blog.kakaocdn.net/dn/bL8BSR/btqZyvHvKfR/01FRVvCkCRbvQYasnmK5q0/img.png)

----------

## 그외의 요청정보 출력하기

```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>info</title></head>");
		out.println("<body>");

		String uri = request.getRequestURI();
		StringBuffer url = request.getRequestURL();
		String contentPath = request.getContextPath();
		String remoteAddr = request.getRemoteAddr();
		
		
		out.println("uri : " + uri + "<br>");
		out.println("url : " + url + "<br>");
		out.println("contentPath : " + contentPath + "<br>");
		out.println("remoteAddr : " + remoteAddr + "<br>");
		
		out.println("</body>");
		out.println("</html>");
	}
```

![](https://blog.kakaocdn.net/dn/IVoaj/btqZvNIFTbs/tXHBrhUgZGbYEfF8sjqfB0/img.png)
*로컬 서버이기때문에 0:0:0:0:0:0:0:1*