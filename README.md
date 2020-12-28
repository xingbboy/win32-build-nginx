## 编译windows下Nginx方法
	这里是已经创建好的工程文件,包含所有用到的库,但需要编译
	
## 下载nginx源码并解压
	http://nginx.org/en/download.html

## 编译zlib
	zlib-1.2.11\contrib\vstudio\目录里保存各个vs版本工程文件(我是vs2017,拷贝了14打开的)
	右键单击zlibvc项目->属性->C/C++->预处理器->预处理器定义->编辑,将ZLIB_WINAPI替换为ZLIB_DLL
	编译win32的release版本
	拷贝zlibwapi.dll,zlibwapi.lib到nginx的libs(没有需要创建目录)里

## 编译openssl
	安装Perl(https://www.perl.org/)
	vs自带有命令提示符.我装的是vs2017,vs 2017命令提示符
	进入openssl-1.1.1i目录
	运行:perl Configure VC-WIN32 no-asm
	运行:ms\do_nt.bat
	运行:nmake -f ms\ntdll.mak
	拷贝libeay32.dll,libeay32.lib,ssleay32.lib到nginx的libs(没有需要创建目录)里

## 编译pcre(已做,直接打开build\pcre.sln编译)
	把config.h.generic重命名成config.h
	把pcre.h.generic重命名成pcre.h
	重命名pcre_chartables.c.dist为pcre_chartables.c
	用vs创建个静态库
	将config.h,pcre_internal.h,pcre_chartables.c,pcre_compile.c,
	pcre_exec.c,pcre_fullinfo.c,pcre_globals.c,pcre_newline.c
	pcre_study.c,pcre_tables.c,pcre_ucd.c文件加入工程
	编译,然后拷贝pcre.lib到nginx的libs(没有需要创建目录)里

## 编译nginx
### 步骤1(已生成 可略过): 主要为了生成ngx_auto_config.h文件
	下载msys2(https://www.msys2.org/)并安装
	启动msys2
	用cd命令进入nginx目录(切换盘符:cd /盘符/)
	在http://nginx.org/en/docs/howto_build_on_win32.html查看配置命令,在Run configure script:下面
	在msys2里运行命令,会在objs里生成文件
	将ngx_auto_config.h,ngx_auto_headers.h,ngx_modules.c,ngx_pch.c拷贝到src目录下

### 步骤2:
	用vs创建个控制台应用程序
	将nginx的src里面的core,event,event\modules,http,http\modules,os\win32添加到工程里
	附加库目录添加  ./libs
	连接器里加入ws2_32.lib,libeay32.lib,ssleay32.lib,pcre.lib,zlibwapi.lib
	编译,这里会出现错误,因为包含linux文件,直接从项目中排除就可以了
