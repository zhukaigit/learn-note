## 软件获取

- svn server下载网址
  http://www.visualsvn.com
- svn client下载网址
  http://www.tortoisesvn.net/downloads

## 服务器端配置

### 中央仓库创建

1. `svnadmin create 中央仓库路径`

### 权限配置

1. 修改shop/conf/svnserve.conf文件

### 服务器启动

1. 命令方式
   `svnserve -d -r 中央仓库路径`
2. 服务注册方式（windows系统）
   以管理员方式运行如：`sc create 服务名称（自定义） binpath="e:\programs\Subversion\bin\svnserve.exe --service -r 中央仓库路径" start=auto depend=Tcpip`





### 认证授权

1. 修改svnserve.conf文件

   1. 注释匿名用于的可读写权限，即注释掉`anon-access = write`
   2. 开启`passord-db`与`auth-db`两项

2. 编写passwd文件
   添加用户名与密码

3. 编写授权文件，即auth文件

   ```
   [groups]
   # harry_and_sally = harry,sally
   # harry_sally_and_joe = harry,sally,&joe
   developer = zhangSan,liSi  #左边是组名，右边为用户名。此组为开发组
   test = wangwu	#左边是组名，右边为用户名。此组为测试组
   
   # [/foo/bar]
   # harry = rw
   # &joe = r
   # * =
   
   # [repository:/baz/fuz]
   # @harry_and_sally = rw
   # * = r
   [Shop:/]
   @developer = rw  #开发组拥有读写权限
   @test = r	#测试组拥有读的权限
   * = r	#匿名用于也拥有可读权限
   ```

