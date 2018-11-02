[第一个Servlet ](#top) <span id="top"></span>  	:maple_leaf:
-----
`JSP的底层其实就是 Servlet 现在我们来跑一个Servlet 吧`
* `新建一个 java web 项目 然后在src 中添加这个类`
* `更改xml 配置文件`
```JAVA
package com.study.jsp;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyServlet extends HttpServlet {

	@Override
	protected void service(HttpServletRequest req,HttpServletResponse resp)
	throws ServletException,IOException
	{
		
		resp.setContentType("text/html;charset=UTF-8");
		String re_str = "<h2>我是你的第一个 Servlet类 </h2>";
		resp.getWriter().write(re_str);
		System.out.println(re_str);
	}
}
```
