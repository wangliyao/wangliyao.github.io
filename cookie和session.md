##关于cookie和session的区别
* 首先cookie的特性:cookie是存于用户浏览器中的，特点是不会占用系统资源，存在时间短，cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session。
* session会在一定时间内保存在服务器上.当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用COOKIE。
* 其实session生存会将key信息保存到cookie中，value保存在service中，session使用时，会去通过cookie中所存的信息匹配service中存的信息，所以seesion是相对于
  cookie来说比较安全的，而且一旦用户禁用cookie，就会导致系统问题
* 在相对不重要的信息可以保存在cookie中，重要的信息，如登录信息，则是保存在session中
