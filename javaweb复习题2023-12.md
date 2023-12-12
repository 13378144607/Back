## 1.单选题

1. ### 关于session保存数据的位置，说法正确的是（B）

    ```
    A、数据保存在客户端
    B、数据保存在服务器端
    C、数据保存在客户端与服务器端各一份
    D、以上说法都不对
    ```

    

1. 在HttpServletRequest接口中，getParameterValues(String name)方法的返回值类型是（B）

    ```
    A、Object[]
    B、String[]
    C、String
    D、Object
    ```

1. 在JSP中， out隐式对象所对应的类是（C）

    ```
    A、Writer
    B、PrintWriter
    C、JspWriter
    D、Print
    ```

1. 下面选项中，用于强制使Session对象无效的方法是（D）

    ```
    A、request. invalidate ();
    B、session. validate ();
    C、response. invalidate ();
    D、session. invalidate ();
    ```

1. 下列选项中，哪个是服务器向客户端发送Cookie的本质？（ A）

    ```
    A、在HTTP响应头字段中增加Set-Cookie响应头字段
    B、在HTTP响应头字段中增加Cookie响应头字段
    C、在HTTP请求头字段中增加Cookie响应头字段
    D、在HTTP请求头字段中增加Set-Cookie响应头字段
    ```

1. 使用@WebServlet注解配置Servlet，设置Servlet对外访问的虚拟路径，应使用哪项属性（D）

    ```
    A、name 
    B、description
    C、displayName
    D、urlPatterns
    ```

1. 下列选项中，哪个 HTML 元素中可以放置 Javascript 代码？（A ）

    ```
    A、<script>
    B、<javascript>
    C、<js>
    D、<scripting>
    ```

1. 下面选项中，哪个方法可以生成一个Cookie对象？（A）

    ```
    A、Cookie c = new Cookie(“name”,”itcast”);
    B、Cookie c = request.getCookie(“name”);
    C、Cookie c = response.getCookie(“name”);
    D、Cookie c = session.getCookie(“name”);
    ```

1. HttpServletRequest接口中，用于获取请求行完整URL的方法是？（ C）

    ```
    A、getMethod()
    B、getRequestURI()
    C、getRequestURL()
    D、getProtocol()
    ```

1. 如果想要将页面传递来的用户名username为张三的数据存放在Requset对象中，以下哪种方式可以实现（C）

    ```
    A、String username=request.getParameter("张三");
    B、String username=(String) request.getAttribute("张三");
    C、request.setAttribute("username", "张三");
    D、request.removeAttribute("张三");
    ```

1. 在HttpServletRequest接口中，用于获取一个指定头字段值的方法是（B ）

    ```
    A、getMethod()
    B、getHeader(String name)
    C、getProtocol()
    D、getHeaderNames(String name)
    ```

1. 下列选项中，能够返回与当前HttpSession对象关联的会话标识号的方法是（C）

    ```
    A、request.getSession();
    B、request.getId();
    C、session.getId();
    D、response.getId();
    ```

1. jsp文件转换后的Servlet源码和class文件所存放的目录是（B）

    ```
    A、Tomcat安装目录/conf/Catalina/localhost/应用名/
    B、Tomcat安装目录/work/Catalina/localhost/应用名/
    C、Tomcat安装目录/lib/Catalina/localhost/应用名/
    D、Tomcat安装目录/webapps/Catalina/localhost/应用名/
    ```

1. 下列选项中，客户端在一次会话过程中可以发送的请求次数是（D）

    ```
    A、1次
    B、0次
    C、2次
    D、多次
    ```

1. 下面选项中，Servlet配置中代表当前Web应用程序的根目录的是（D）

    ```
    A、\
    B、//
    C、*
    D、/
    ```

1. 下列关于<servlet-mapping>作用的说法中，正确的是（A）

    ```
    A、用于映射Servlet对外访问的虚拟路径
    B、指定Servlet类的路径
    C、设置Servlet名称
    D、以上都不对
    ```

1. 下列选项中，关于Cookie中保存的数据，说法正确的是（ B）

    ```
    A、保存在服务器中
    B、保存在客户端浏览器中
    C、保存在数据库中
    D、以上说法都不对
    ```

1. 下面选项中，用于存放Tomcat startup.bat和shutdown.bat文件的是（A）

    ```
    A、bin
    B、conf
    C、lib
    D、webapps
    ```

1. 下面选项中，通常将要发布的应用程序放到Tomcat主目录哪个子目录内（D）

    ```
    A、bin
    B、conf
    C、lib
    D、webapps
    ```

1. HttpServlet类中，用来处理POST请求的方法是（C）

    ```
    A、doHead
    B、doGet
    C、doPost
    D、doPut
    ```

1. 在HttpServletRequest接口中，用于获取指定名称请求参数值的方法是（D）

    ```
    A、Object getParameter(Object name)
    B、Object getParameter(String name)
    C、String getParameter(Object name)
    D、String getParameter(String name)
    ```

1. 下面选项中，属于HttpServletResponse接口中定义的用于实现请求重定向的方法是（C）

    ```
    A、Redirect()
    B、send ()
    C、sendRedirect()
    D、forward()
    ```

1. 以下选项中，不属于Servlet生命周期中初始化阶段、运行阶段、销毁阶段的方法是（B）。

    ```
    A、init方法
    B、main方法
    C、service方法
    D、destroy
    ```

1. 下面选项中，用于获取与当前请求相关的HttpSession对象的语句是（A）

    ```
    A、request.getSession();
    B、response.getSession(false);
    C、new HttpSession();
    D、HttpSession.newInstance(request);
    ```

1. 下列选项中，属于sendRedirect(java.lang.String url)方法所在接口的是（ C）

    ```
    A、HttpSession
    B、HttpServletRequest
    C、HttpServletResponse
    D、ServletResponse
    ```

1. LoginServlet是一个Servlet类，代码如下public class LoginServlet extends ___________ {  public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    …  }  public void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    …  }} 在划线处应填写（B）

    ```
    A、Servlet
    B、HttpServlet
    C、GenericServlet
    D、Cookie
    ```

1. 在HttpServletRequest接口中，用于获取请求行中的协议名和版本的方法是（D）

    ```
    A、getMethod()
    B、getRequestURI()
    C、getQueryString()
    D、getProtocol()
    ```

1. HttpServletResponse接口中用于设置Servlet输出内容的MIME类型的方法是（C）

    ```
    A、setContent (String type)
    B、setContentLength(int type)
    C、setContentType(String type)
    D、setType(String type)
    ```

1. 在HttpServletRequest接口中，用于获取指定名称请求参数的一组值的方法是（B）

    ```
    A、Object[] getParameterValues(String name)
    B、String[] getParameterValues(String name)
    C、String getParameter(Object name)
    D、String[] getParameters(String name)
    ```

1. 下面选项中，用于设置当前HttpSession对象可空闲的以秒为单位的最长时间的方法是（D）

    ```
    A、request. setMaxInactiveInterval ();
    B、request. getCreationTime();
    C、response. setMaxInactiveInterval ();
    D、session. setMaxInactiveInterval ();
    ```

1. 将jsp转换成的Servlet后，用户访问JSP文件时会被调用的方法是（C）

    ```
    A、_jspInit()
    B、_jspDestroy()
    C、_jspService()
    D、Serivce()
    ```

1. HttpServletRequest接口中getParameterNames()方法的返回值类型是（C）

    ```
    A、Object[]
    B、String[]
    C、Enumeration
    D、Object
    ```

1. RequestDispatcher接口中能实现请求包含的方法是（B）

    ```
    A、forward(ServletRequest request，ServletResponse response)
    B、include(ServletRequest request，ServletResponse response)
    C、include(ServletResponse response，ServletRequest request)
    D、sendRedirect(String url)
    ```

1. HttpServletResponse接口中用于设置输出内容使用的字符编码的方法是（D）

    ```
    A、setEncodingCharacter(String charset)
    B、setEncoding(String charset)
    C、setCharacter (String charset)
    D、setCharacterEncoding(String charset)
    ```

1. HttpServletResponse对象中用于获取字节输出流对象的方法是（B）

    ```
    A、getStream()
    B、getOutputStream()
    C、getOutput()
    D、getWriter()
    ```

1. 下列对于setMaxAge(-1)方法的描述中，正确的是（C）

    ```
    A、表示通知浏览器保存这个Cookie信息
    B、表示通知浏览器立即删除这个Cookie信息
    C、表示当浏览器关闭时，Cookie信息会被删除
    D、以上都不正确
    ```

1. 下面选项中，用于封装JSP中抛出的异常信息的隐式对象是（D）

    ```
    A、page
    B、out
    C、request
    D、exception
    ```

1. 在HttpServletRequest接口中用于获取HTTP请求消息中的请求方式的方法是（A）

    ```
    A、getMethod()
    B、getRequestURI()
    C、getQueryString()
    D、getProtocol()
    ```

1. RequestDispatcher接口中，用于将请求从一个Servlet转发给另外的一个Web资源、且只由目标资源进行响应的方法是（C）

    ```
    A、forward(ServletResponse response，ServletRequest request)
    B、include(ServletRequest request，ServletResponse response)
    C、forward(ServletRequest request，ServletResponse response)
    D、include(ServletResponse response，ServletRequest request)
    ```

1. 当Servlet发送响应消息时，需要在响应消息中设置（B）

    ```
    A、验证码
    B、状态码
    C、请求码
    D、MD5密码
    ```

1. JSP程序中获取JavaBean的实例，应使用下面哪个动作元素（A）

    ```
    A、<jsp:useBean>
    B、<jsp:setProperty>
    C、<jsp:getProperty>
    D、<jsp:bean>
    ```

1. Filter接口中doFilter方法的参数列表，下面正确的是哪一项？（C）

    ```
    A、doFilter(ServletRequest request , ServletResponse response) 
    B、doFilter(FilterChain filterChain)
    C、doFilter(ServletRequest request , ServletResponse response , FilterChain filterChain)
    D、doFilter(FilterChain filterChain , ServletRequest request , ServletResponse response)
    ```

1. 过滤器使用过滤器链filterChain对请求放行的语句应为（C）

    ```
    A、filterChain.do (request,response)
    B、filterChain.filter(request,response)
    C、filterChain.doFilter(request,response)
    D、filterChain.doFilter(response ,request)
    ```

1. FilterChain接口中doFilter方法的参数列表，下面正确的是哪一项？（A）

    ```
    A、doFilter(ServletRequest request , ServletResponse response)
    B、doFilter(FilterChain filterChain)
    C、doFilter(ServletRequest request , ServletResponse response , FilterChain filterChain)
    D、doFilter(FilterChain filterChain , ServletRequest request , ServletResponse response)
    ```

1. 过滤器链filterChain调用doFilter方法对请求放行，对放行语句前后代码功能的描述正确的是（A）

    ```
    A、filterChain.doFilter(request,response)语句前的代码为预处理、后的为后处理
    B、filterChain.doFilter(request,response)语句前、后的代码均为预处理
    C、filterChain.doFilter(request,response)语句前、后的代码均为后处理
    D、filterChain.doFilter(request,response)语句前的代码为后处理、后的为预处理
    ```

1. 获取数据库连接，应使用下面哪条语句（A）

    ```
    A、DriverManager.getConnection(String url , String user , String password)
    B、DriverManager.getConnection(String user , String password , String url)
    C、DriverManager.getConnection(String password, String user , String url)
    D、以上都不对
    ```

1. 各种数据库的JDBC驱动类，是以下那个接口的实现类（C）

    ```
    A、javax.sql.Driver
    B、com.mysql.jdbc.Driver
    C、java.sql.Driver
    D、java.sql.DriverManager
    ```

1. 载入数据库的JDBC驱动程序，应使用下面哪条语句（B）

    ```
    A、class.forName("DriverName"); 
    B、Class.forName("DriverName");
    C、class.forname("DriverName");
    D、 Class.ForName("DriverName");
    ```

1. 表示数据库连接的，是以下那个接口的实例（B）

    ```
    A、java.sql.Driver
    B、java.sql.Connection
    C、java.sql.Statement
    D、java.sql.ResultSet
    ```

1. Java程序执行SQL语句，下面两个接口描述正确的是（A）

    ```
    A、Statement接口执行静态SQL语句；PreparedStatement接口可执行带参数的预编译SQL语句
    B、Statement接口和PreparedStatement接口都只能执行静态SQL语句
    C、Statement接口和PreparedStatement接口都可以执行带参数的预编译SQL语句
    D、Statement接口执行带参数的预编译SQL语句；PreparedStatement接口执行静态SQL语句
    ```


## 2.多选题

1. 下面关于HTTP协议的说法中，正确的是（ABCD）(多选)

    ```
    A、HTTP是Hyper Text Transfer Protocol的缩写，即超文本传输协议。
    B、HTTP是一种请求/响应式的协议
    C、客户端向服务器端发送一个请求，被称作HTTP请求消息
    D、服务器端接收到请求后会做出响应，称为HTTP响应消息
    ```

1. 一个用户安装了Tomcat，但无法启动Tomcat，可能是由于哪些原因引起的？（ABCD）（多选）

    ```
    A、没有安装JDK
    B、Tomcat与JDK的版本不匹配
    C、没有设置JAVA_HOME系统环境变量
    D、Tomcat服务器所使用的网络监听端口被其它服务程序占用
    ```

1. 表单method属性设置表单数据提交方式，关于GET和POST方式的描述中，哪些是正确的（BD）。

    ```
    A、使用GET方式提交的数据没有大小限制
    B、使用POST方式提交的数据没有大小限制
    C、使用GET方式提交的数据在地址栏中不会显示
    D、使用POST方式提交的数据在地址栏中不会显示
    ```

1. 下列选项中能正确实现一个servlet的方式有（ABC）（多选）

    ```
    A、继承javax.servlet.http.HttpServlet类
    B、实现javax.servlet.Servlet接口
    C、继承javax.servlet.GenericServlet 类
    D、自定义一个类，命名为Servlet
    ```

1. HttpServlet中定义的doGet和doPost方法的参数类型有哪些？（BC） 

    ```
    A、ServletRequest
    B、HttpServletRequest
    C、HttpServletResponse
    D、ServletResponse
    ```

1. 下列异常中，哪些是在Servlet中重写doGet(),doPost()方法时抛出的异常（AD） 

    ```
    A、ServletException
    B、HttpServletException
    C、HttpException
    D、IOException
    ```

1. 下列选项中，属于HttpServletResponse类中发送状态码的方法是（AD）（多选）

    ```
    A、setStatus(int status)
    B、setStatus(String status)
    C、setError(String status)
    D、setError(int status)
    ```

1. HttpServletResponse接口中用于设置各种头字段的方法是（ABCD）（多选）

    ```
    A、addHeader()
    B、setHeader()
    C、addIntHeader()
    D、setIntHeader()
    ```

1. 下列选项中，属于RequestDispatcher接口中方法的有（BC）（多选）

    ```
    A、sendRedirect()方法
    B、include()方法
    C、forward()方法
    D、dispatcher()方法
    ```

1. 下列关于请求转发和请求重定向的说法中，正确的是（ABCD）（多选）

    ```
    A、请求转发和请求重定向都可以实现访问一个资源时转向新的目标资源
    B、请求转发时客户端不必重发请求，而请求重定向需要客户端重发请求
    C、一般情况下应该使用请求转发，减少浏览器对服务器的访问，减轻服务器压力
    D、如果需要改变浏览器的地址栏，应使用请求重定向
    ```

1. 下列选项中，属于Servlet常用会话技术的是（AB）

    ```
    A、Cookie
    B、Session
    C、Application
    D、request
    ```

1. 下面关于HttpSession的说法中，正确的是（AB） （多选）

    ```
    A、Servlet容器负责创建HttpSession对象
    B、每个HttpSession对象都有唯一的ID
    C、客户端程序浏览器负责为HttpSession分配唯一的ID
    D、JSESSIONID需要保存在服务器
    ```

1. 
    给定一个Servlet的代码片段如下所示：

    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{

    ​     String str = "world";

    ​     HttpSession session = request.getSession();

    ​     session.setAttribute("str", str);

    ​     ___________________

    }    

    要取出session中的值，下划线处的代码可以是（BC） （多选）

    ```
    A、String s = session.getParameter("str");
    B、String s = (String)session.getAttribute("str");
    C、Object o = session.getAttribute("str");
    D、Object o = (Object)session.getParameter("str");
    ```

1. 下面关于JSP的说法中，正确的是（ABCD）

    ```
    A、它是建立在Servlet规范之上的动态网页开发技术
    B、它的代码由HTML代码与Java代码组成
    C、.jsp文件中， HTML代码用来实现网页中静态内容的显示
    D、.jsp文件中， Java代码用来实现网页中动态内容的显示
    ```

1. JavaBean应具备的特点（ABCD）

    ```
    A、属性应修饰为private
    B、必须提供一个public、无参数的构造方法
    C、对于名为xxx的属性，应提供public的、名为getXxx()和setXxx()的方法
    D、对于boolean型属性，可以使用以上get和set方式，也可以把get替换成 is
    ```

1. 关于MVC设计模式，下列描述错误的是（BCD）

    ```
    A、M指模型，V指视图，C指控制器
    B、Model模型通过定义JSP程序实现
    C、View视图通过定义Servlet程序实现
    D、Controller控制器通过定义JavaBean实现
    ```

1. 下面选项中，属于过滤器Filter接口中包含的方法有（ABC）

    ```
    A、init(FilterConfig filterConfig)
    B、doFilter(ServletRequest req,ServletResponse resp,FilterChain chain)
    C、destroy()
    D、service(ServletRequest req,ServletResponse resp,FilterChain chain)
    ```

1. 过滤器Filter生命周期中的方法，下列描述正确的是（ACD）

    ```
    A、init初始化方法：Tomcat创建Filter实例后，自动调用该方法1次
    B、doFilter方法完成实际的过滤操作，在过滤器生命周期中只能被调用1次
    C、doFilter方法完成实际的过滤操作，在过滤器生命周期中可以被调用多次
    D、destroy销毁方法：Tomcat销毁Filter实例前，自动调用该方法1次
    ```

1. JSP中使用动作元素<jsp:useBean>获取JavaBean实例时，属性scope的取值可以是（ABCD）

    ```
    A、page
    B、request
    C、session
    D、application
    ```

1. 对JDBC功能的描述，下面正确的是（ABC）

    ```
    A、连接数据库
    B、执行SQL语句
    C、处理SQL语句的执行结果
    D、以上都错
    ```


## 3.判断题

1. Servlet API中提供了一个javax.servlet.http.Cookie类，Cookie类有且仅有一个构造方法。（V）

1. Servlet的多重映射指的是同一个Servlet可以被映射成多个虚拟路径。(V)

1. page指令的属性中，所有的属性都只能出现一次，否则会编译失败。(X)

1. 服务器向客户端发送Cookie时，会在HTTP响应头字段中增加Set-Cookie响应头字段。(V)

1. JSP全名是Java Server Pages，它是一套全新的技术，与Servlet完全没有任何联系。(X)

1. 如果请求消息中没有包含指定名称的请求参数，则getParameter()方法返回null。(V)

1. 一个<servlet-mapping>元素下配置多个<url-pattern>子元素能实现Servlet的多重映射。(V)

1. Session是一种将会话数据保存到服务器端的技术，需要借助Cookie技术来实现。(V)

1. 在Servlet的一个生命周期中，其init方法和destory方法都可以被多次调用。(X)

1. 通过请求转发来实现目标资源的访问是服务器内部的行为，对于客户端来说是一次请求过程。(V)

1. 编写完过滤器的类之后，不需要对该过滤器进行任何配置，就可以让其拦截请求的资源。(X)

1. HttpServletRequest接口中的getParameterValues(String name)方法返回一个String[ ] 数组。(V)

1. 所谓请求重定向，指的是Web服务器接收到客户端的请求后，又再次引导客户端重新发送请求，指定了一个新的资源路径。(V)

1. 在Servlet技术中，提供了两个用于保存会话数据的对象，分别是Cookie和Session。(V)

1. 在一个Cookie对象中，如调用setMaxAge()方法设置值为负整数，当浏览器关闭时，Cookie信息会被删除。(V)

1. 用户每次访问JSP页面时，该页面都会被JSP容器转换成一个Servlet源文件，然后将源文件编译为.class文件。(X)

1. HTML语言主要是通过HTML标记对网页中的文本、图片、声音等内容进行描述。(V)

1. 在.jsp中使用HTML注释，可以在客户端浏览器生成动态注释。(V)

1. 元素<url-pattern>是用于指定访问该Servlet的虚拟路径，该路径以正斜线(/)开头，代表当前Web应用程序的根目录。(V)

1. JSP文件与html文件一样，在编写好后都可以直接在浏览器中运行。(X)

1. 在HTML中使用注释标记时，注释内容不会显示在浏览器窗口中。(V)

1. JavaScript代码只能嵌入在HTML中。(X)

1. 在 HTML文档中引入JavaScript，有直接嵌入JavaScript脚本和链接外部JavaScript脚本两种。(V)

1. 安装好Tomcat后，就可以直接启动运行了，并不要先安装JDK。(X)

1. 在Servlet程序中，只有属于同一个请求中的数据才可以通过HttpServletRequest对象传递。(V)

1. HTML表单元素的method属性用于设置表单数据提交方式，其默认值为post(X)

1. 狭义的Servlet指javax.servlet.Servlet接口；广义的Servlet指任何实现了该接口的类。(V)

1. javax.serlvet.Servlet接口中只定义了1个抽象方法，方法名为service。(X)

1. ==GenericServlet类实现了Servlet接口中的所有方法。(X)==

     ```doc
     GenericServlet类实现了Servlet接口，但没有实现Servlet接口中的所有方法。GenericServlet是一个抽象类，它提供了Servlet接口的默认实现，但留下了service方法需要具体的子类去实现。所以，虽然提供了通用的Servlet支持，但它并没有完全实现Servlet接口中的所有方法。
     ```

     

1. 在HttpServlet类中，与GET请求方式对应的是doPost()方法，即请求方式为GET，doPost()方法将被执行。(X)

1. Tomcat初始化一个Servlet时，会将该Servlet的配置信息封装到一个ServletConfig对象中。(V)

1. ==TomCat启动时，会为每个Web应用创建ServletContext对象代表当前Web应用，且每个Web应用可以有多个ServletContext对象。(X)==

     ```doc
     Tomcat启动时，会为每个Web应用创建一个ServletContext对象代表当前Web应用。每个Web应用只有一个ServletContext对象，这个对象是用来在整个应用中共享信息的。不同的Servlet和组件可以通过ServletContext对象进行通信。多个ServletContext对象通常不会被用于同一个Web应用。
     ```

     

1. ==$\textcolor{red}{HttpServletResponse}$专门用来封装Http$\textcolor{red}{请求消息}$。(X)==

     ```doc
     HttpServletResponse专门用来封装Http响应消息，而不是请求消息。它提供了一种Servlet可以使用的方法，以便向客户端发送Http响应，包括设置响应的状态码、头部信息和内容等。请求消息则是通过HttpServletRequest来封装和访问的。
     ```

1. Servlet编程中，实现请求重定向，需要调用HttpServletRequest中的sendRedirect()方法。(X)

1. RequestDispatcher对请求进行包含，需要调用forward()方法。(X)

1. 多个过滤器对同一个URL请求进行过滤，这些过滤器就组成一个过滤器链。(V)

1. **一个过滤器只能对一个URL请求进行过滤，不能过滤多个URL请求。**(X)

1. **JDBC指Java DataBase Connectivity，即Java数据库连接。**(V)

1. **结果集ResultSet对象初始化后，游标（指针）即指向表格的第一行。**(X)

1. **每次操作数据库结束后不必关闭数据库连接、不应释放资源。**(X)

    