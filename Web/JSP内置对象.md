## JSP的隐式内在对象有9个
- request 用户请求
- response 网页回传用户端的响应
- out 用来传送回应的输出
- pageContext 当前页面作用域
- session 会话作用域
- application 全局作用域
- page 当前对象，JSP会被编译为一个Servlet类，page即代表this
- config
- exception 针对错误网页，异常对象。