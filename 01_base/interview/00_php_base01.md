

\r \n \t \x20
------------
\n 换行(LF)，将当前位置移到下一行开头  
\r 回车(CF)，将当前位置移到本行开头  
\t 水平制表符(HT)，相当于按一次tab键  

\x20是32在ASCII表中16进制的表示。  

以下语句输出的结果是什么
------------
```
$a = 3;  
echo "$a",'$a',"\\\$a","${a}","$a"."$a","$a"+"$a";  
得到的结果是：3$a\$a3336  
```

以下语句输出的结果是什么
------------
```
setcookie("a","value");
print $_COOKIE['a'];
结果：
PHP Notice: Undefined index: a
```

什么是魔术引号magic_quotes
------------
魔术引号（Magic Quotes）是一个自动将进入 PHP 脚本的数据进行转义的过程。如：post、get、cookie过来的数据增加转义字符“\”，以确保这些数据不会引起程序，特别是数据库语句因为特殊字符引起的污染而出现致命的错误。

在php配置文件中 magic_quotes_gpc = On的情况下，如果输入的数据有

单引号（’）、双引号（”）、反斜线（\）与 NULL（NULL 字符）等字符都会被加上反斜线。这些转义是必须的，如果这个选项为Off，那么我们就必须调用addslashes这个函数来为字符串增加转义。

正是因为这个选项必须为On，但是又让用户进行配置的矛盾，在PHP6中删除了这个选项，一切的编程都需要在 magic_quotes_gpc=Off下进行了。在这样的环境下如果不对用户的数据进行转义，后果不仅仅是程序错误而已了。同样的会引起数据库被注入攻击的危险。所以从现在开始大家都不要再依赖这个设置为On了。

我们可以通过以下代码来探测php环境中magic_quotes_gpc是否开启，如：  
```
<?php
//当magic_quotes_gpc=On的时候，get_magic_quotes_gpc函数的返回值为1
//当magic_quotes_gpc=Off的时候，get_magic_quotes_gpc函数的返回值为0

if (get_magic_quotes_gpc()) {
	echo 'magic_quotes_gpc 开启';
} else {
	echo 'magic_quotes_gpc 未开启';
}
```


字符串怎么转成整数，有几种方法？怎么实现？
-------------
强制类型转换:   
(整型)字符串变量名;   
intval(字符串变量);    
直接转换：settype(字符串变量, 整型);    


标量数据和数组的最大区别是什么？
------------
一个标量只能存放一个数据，而数组可以存放多个数据。


常量和变量有哪些区别？
------------
1）常量前没有$符号；  
2）常量只能通过define()定义，而不能通过赋值语句定义；  
3）常量可以在任何地方定义和访问，而变量有全局和局部之分；  
4）常量一旦定义就不能被重新定义或者取消定义，而变量而通过赋值方式重新定义；  
5）常量的值只能是标量数据，而变量的数据库类型有8种原始数据类型。  


常量如何定义? 如何检测一个常量是否被定义？常量的值只能是哪些数据类型？
------------
define()//定义常量 , defined()//检查常量是否定义
常量的值只能是标量类型的数据。

如果定义了两个相同的常量，前者和后者哪个起作用？
------------
前者起作用，因为常量一旦定义就不能被重新定义或者取消定义。

在实际开发中，常量最常用于哪些地方？
------------
1）连接数据库的信息定义成常量，如数据库服务器的用户名、密码、数据库名、主机名；
2）将站点的部分路径定义成常量，如web绝对路径，smarty的安装路径，model、view或者controller的文件夹路径；
3）网站的公共信息，如网站名称，网站关键词等信息。


常量分为系统内置常量和自定义常量。请说出最常见的几个系统内置常量？
------------
__FILE__ , __LINE__ , PHP_OS , PHP_VERSION


PHP中常用的几个预定义的全局数组变量是哪些？
-----------
有9大预定义的内置数组变量：
$_POST, $_GET, $_REQUEST, $_SESSION, $_COOKIE, $_FILES，$_SERVER, $_ENV, $GLOBALS


$_SERVER 常用值
------------
```
$_SERVER['REMOTE_ADDR']; 显示客户端IP的预定义变量

$_SERVER['SERVER_ADDR'] #服务器ip  
$_SERVER['SERVER_PORT'] #服务器所使用的端口  

$_SERVER['HTTP_REFERER']; 提供来路url

$_SERVER['REMOTE_HOST'] //当前用户主机名; 

$_SERVER['REQUEST_METHOD']//访问页面时的请求方法

$_SERVER['SCRIPT_FILENAME'] #当前执行脚本的绝对路径名。

$_SERVER['PHP_SELF']//正在执行脚本的文件名，带路径从web根目录起 

$_SERVER['QUERY_STRING'] //查询(query)的字符串。 


# 例子
#===============
#测试网址:     http://localhost/blog/testurl.php?id=5

//获取域名或主机地址 
echo $_SERVER['HTTP_HOST']."<br>"; #localhost

//获取网页地址 
echo $_SERVER['PHP_SELF']."<br>"; #/blog/testurl.php

//获取网址参数 
echo $_SERVER["QUERY_STRING"]."<br>"; #id=5

//获取用户代理 
echo $_SERVER['HTTP_REFERER']."<br>"; 

//获取完整的url
echo 'http://'.$_SERVER['HTTP_HOST'].$_SERVER['REQUEST_URI'];
echo 'http://'.$_SERVER['HTTP_HOST'].$_SERVER['PHP_SELF'].'?'.$_SERVER['QUERY_STRING'];
#http://localhost/blog/testurl.php?id=5

//包含端口号的完整url
echo 'http://'.$_SERVER['SERVER_NAME'].':'.$_SERVER["SERVER_PORT"].$_SERVER["REQUEST_URI"]; 
#http://localhost:80/blog/testurl.php?id=5

//只取路径
$url='http://'.$_SERVER['SERVER_NAME'].$_SERVER["REQUEST_URI"]; 
echo dirname($url);
#http://localhost/blog
```

php把utf-8转换成gbk的函数是
-----------
iconv(‘UTF-8′,’GBK’,$str)  


$a = 1; $b = &$a;  
unset($a),$b是否还是1，为什么？  
unset($b),$a是否还是1，为什么？  
-----------
都等于1。

在php中，引用赋值不同于指针的感念，他只是将另一个变量名指向了某个内存地址。此题中:$b = &$a;只是将$b这个名字也指向了$a变量所指向的内存地址。unset时只是释放了这个名字的指向，并没有释放内存中的值。

另一方面讲unset($a),其实也并未真正立刻释放内存中的值，也只是释放了这个名字的指向而已，该函数只有在变量值所占空间超过256字节长的时候才会释放内存，并且只有当指向该值的所有变量（比如有引用变量指向该值）都被销毁后，地址才会被释放。


变量赋值方式有哪几种？
------------
1）直接赋值  
2）变量间赋值  
3）引用赋值  


php中函数传递参数的方式有哪些？两者有什么区别？
------------
按值传递 和 按地址传递（或按引用传递）   
(1)按值传递: 待传递的变量和用于接收的变量只是值相同，但是存储在不同的空间中。所以函数体内对该变量值做的修改，不影响原本的变量值。  

(2)按地址传递: 使用&符号，表明该参数是以地址的方式传递值。
这种方递方法并不会将主程序中的指定数值或目标变量传递给函数，而是把该数值或变量的内存储存区块地址导入函数之中，所以函数体内的该变量和主程序中的该变量在内存中是同一个。函数体做的修改，直接影响到函数体外部的该变量的值。  

引伸思考：变量在内容中到底是怎么存储的


引用和拷贝有什么区别？(同上题差不多)
------------
拷贝是将原来的变量内容复制下来，拷贝后的变量与原来的变量使用各自的内存，互不干扰。  
引用相当于是变量的别名，其实就是用不同的名字访问同一个变量内容。当改变其中一个变量的值时，另一个也跟着发生变化。  


什么是静态变量？
------------
如果一个函数内定义的变量前使用关键字static来声明，那么该变量就是静态变量。  
一般函数内的变量在函数调用结束后，其存储的数据将被清除，所占的内存空间也被释放。而使用静态变量时，该变量会在函数第一次被调用时被初始化，初始化后该变量也不会被清除，当再次调用该函数时，这个静态变量不再被初始化，而能保存上次函数执行完后的值。可以说静态变量在所有对该函数的调用之间共享。  


什么是递归函数？如何进行递归调用？
-------------
递归函数其实就是调用自身的函数，但是必须满足以下两个条件：  
1）在每一次调用自身时，必须是更接近于最终结果；  
2）必须有一个确定的递归终止条件，不会造成死循环。  
举例说明：在实际工作中往往会在遍历文件夹的时候使用。  
如果有个例子是希望获取到目录windows下所有的文件，那么先遍历windows目录，如果发现其中还有文件夹，那么就会调用自身，继续往下寻找，依次类推，直到遍历到再也没有文件夹为止，这也就是意味着遍历出来了所有的文件。  


说出前置++和后置++的区别？
-------------
前置++是先将变量增加1，然后在将值赋值给原来的变量；  
后置++是先返回变量的当前值，然后再将变量的当前值增加1。  


字符串运算符“.”与算术运算符“+”有什么区别？
--------------
"."是用来连接两个字符串  
"+"是用来进行做加法运算，  
1）如果是两个数字，直接相加  
2) 如果是字符串，先将字符串自动转为整型再相加  
3）如果是数组，则加两个数组合并成一个数组，合并时或两组存在相同的键名，则只保留第一个数组的，每二数组的抛弃。  


什么是三目（或三元）运算符？
--------------
根据一个表达式的结果在另两个表达式中选择一个。  
例如: $a==true  'good':'bad';  


控制流程语句有哪些？
--------------
1：三种程序结构 顺序结构、分支结构、循环结构  
2：分支： if/esle/esleif/ switch/case/default  
3: switch case 需要注意的：
case子句中的常量可以是整型、字符串型常量、 或者常量表达式，不允许是变量。  
同一个switch子句中，case的值不能相同，否则只能取到首次出现case中的值。  
4: 循环 for while do...while  
注意：do...while 后面必须加入分号结尾。while 和 do...while 的区别  


break 和 continue 的区别。
--------------
break可以终止循环。  
continue没有break强大，只能终止本次循环而进入到下一次循环中。  


foeach数组的时候指针是如何指向的？list()/each()/while()循环数组的时候指针如何指向的呢？
--------------
当foreach开始执行的时候，数组内部的指针会自动指向第一个单元。因为foreach所操作的是指定数组的拷贝，而不是该数组本身。  
而each()一个数组后，数组指针将停留在数组中的下一个单元或者碰到数组结尾时停留在最后一个单元。如果要再次使用each()遍历数组，必须要使用reset()。  
reset()将数组的内部指针倒回到第一个单元并返回第一个数组单元的值。  



如何计算数组长度（或者说计算数组中所有元素的个数）？字符串怎么取长度？
---------------
count() -- 计算数组中的元素个数。  
可以使用count(数组名)或者count(数组名,1)，第二个参数若是1，则表示递归统计数组元素的个数。
若是0，则等同于只有一个参数的count()函数。  
sizeof() 是 count() 的别名  

字符串长度：strlen()、mb_strlen();  



数组中相关的常用函数有哪些？
---------------
1） count --（sizeof别名）— 计算数组中的单元数目或对象中的属性个数   
例如：int count ( mixed $var [, int $mode ] ) $var 通常都是数组类型，任何其它类型都只有一个单元。$mode 默认值为0. 1为开启递归地对数组计数  
2） in_array ( mixed $needle , array $haystack [, bool $strict ] ) — 检查数组中是否存在某个值。
如果 needle 是字符串，则比较是区分大小写的。
如果第三个参数 strict 的值为 TRUE 则 in_array() 函数还会检查 needle 的类型是否和 haystack 中的相同。  
3） array_merge(array $array1 [, array $array2 [, array $... ]] ) 将一个或多个数组的单元合并起来，一个数组中的值附加在前一个数组的后面。返回作为结果的数组。 
特别注意：如果输入的数组中有相同的字符串键名，则该键名后面的值将覆盖前一个值。然而，如果数组包含数字键名，后面的值将不会覆盖原来的值，而是附加到后面。 
如果只给了一个数组并且该数组是数字索引的，则键名会以连续方式重新索引。  



数组与字符串之间的转换
----------------
(1)explode ( string $separator , string $string [, int $limit ] ) 使用一个分隔字符来分隔一个字符串。  
(2)implode ( string $glue , array $arr ) 使用一个连接符将数组中的每个单元连接为一个字符串。  
join 为 implode 的别名  


数组合并函数array_merge()和数组加法运算$arr + $arr2 的区别是什么？
----------------
array_merge()：如果是关联数组合并，如果数组的键名相同，那么后面的值将覆盖前者；如果是数字索引数组合并，则不覆盖，而是后者附加到前者后面。  
"+"：不管是关联数组还是数字索引数组，若存在相同键名，只保留首次出现该键名的元素，后来的具有相同键名的元素都不会被加进来。    


echo(),print(),print_r()的区别？
----------------

print_r可以通过print_r($str, true)来，使print_r不输出而返回print_r处理后的值。此外，对于echo和print，基本以使用echo居多，因为其效率比print要高。



页面字符出现乱码，怎么解决?
----------------
1.首先考虑当前文件是不是设置了字符集。查看是不是meta标签中写了charset，如果是php页面还可以看看是不是在header()函数中指定了charset；  
例如：  
```
<meta charset="utf-8"/>
header("content-type:text/html;charset=utf-8");
```
2.如果设置了字符集（也就是charset），那么判断当前文件保存的编码格式是否跟页面设置的字符集保持一致，两者必须保持统一；  
3.如果涉及到从数据库提取数据，那么判断数据库查询时的字符集是否跟当前页面设置的字符集一致，两者必须统一，例如：mysql_query("set names utf8)。  



正则表达式是什么？php中有哪些常用的跟正则相关的函数？请写出一个email的正则，中国手机号码和座机号码的正则表达式？
-----------------
正则表达式是用于描述字符排列模式的一种语法规则。正则表达式也叫做模式表达式。  
网站开发中正则表达式最常用于表单提交信息前的客户端验证。比如验证用户名是否输入正确，密码输入是否符合要求，email、手机号码等信息的输入是否合法。  
在php中正则表达式主要用于字符串的分割、匹配、查找和替换操作。  

preg系列函数可以处理。具体有以下几个：
string preg_quote ( string str [, string delimiter] )
转义正则表达式字符 正则表达式的特殊字符包括：. \\ + * ? [ ^ ] $ ( ) { } = ! < > | :。
preg_replace -- 执行正则表达式的搜索和替换
mixed preg_replace ( mixed pattern, mixed replacement, mixed subject [, int limit] )
preg_replace_callback -- 用回调函数执行正则表达式的搜索和替换
mixed preg_replace_callback ( mixed pattern, callback callback, mixed subject [, int limit] )
preg_split -- 用正则表达式分割字符串
array preg_split ( string pattern, string subject [, int limit [, int flags]] )



preg_replace()和 str_ireplace()两个函数在使用上有什么不同？preg_split()和split()函数如何使用？
-------------------


url中用get传值的时候，若中文出现乱码，应该用哪个函数对中文进行编码
-------------------
使用urlencode()对中文进行编码，使用urldecode()来解码。



请说出目前学过的返回是资源的函数？
------------------
mysql_connect();  
mysql_query();只有这执行select的时候成功，才返回资源，失败返回FALSE  
fopen();  


文件上传需要注意哪些细节？怎么把文件保存到指定目录？怎么避免上传文件重名问题？
------------------
1.首现要在php.ini中开启文件上传；  
2.在php.ini中有一个允许上传的最大值，默认是2MB。必要的时候可以更改；  
3.上传表单一定要记住在form标签中写上enctype="multipart/form-data"；提交方式 method 必须是 post；  
4.设定 type="file" 的表单控件；  
5.要注意上传文件的大小MAX_FILE_SIZE、文件类型是否符合要求，上传后存放的路径是否存在。  

可以通过上传的文件名获取到文件后缀，然后使用时间戳+文件后缀的方式为文件重新命名，这样就避免了重名。  
可以自己设置上传文件的保存目录，与文件名拼凑形成一个文件路径，使用move_uploaded_file()，就可以完成将文件保存到指定目录。  


$_FILES是几维数组？第一维和第二维的索引下标分别是什么？批量上传文件的时候需要注意什么？
------------------
二维数组。  
第一维是上传控件的name，二维下标分别为name/type/tmp_name/size/error.  


什么是会话控制？
----------------
简单地说会话控制就是跟踪和识别用户信息的机制。会话控制的思想就是能够在网站中跟踪一个变量，通过这个变量，系统能识别出相应的用户信息，根据这个用户信息可以得知用户权限，从而展示给用户适合于其相应权限的页面内容。  
目前最主要的会话跟踪方式有cookie，session。  


会话跟踪的基本步骤
--------------
1)．访问与当前请求相关的会话对象  
2)．查找与会话相关的信息  
3)．存储会话信息  
4)．废弃会话数据  


使用cookie的注意事项有哪些？
----------------
1） setcookie()之前不可以有任何页面输出，就是空格，空白行也不可以；  
2） setcookie()后，在当前页面调用$_COOKIE['cookiename']不会有输出，必须刷新或到下一个页面才可以看到cookie值；  
3） 不同的浏览器对cookie处理不同，客户端可以禁用cookie，浏览器也可以闲置cookie的数量，一个浏览器能创建的cookie数量最多300个，并且每个不可以超过4kb，
每个web站点能设置的cookie总数不能超过20个。  
4） cookie是保存在客户端的，用户禁用了cookie，那么setcookie就不会起作用了。所以不可以过度依赖cookie。  


使用session的时候，通过什么来表示当前用户，从而与其他用户进行区分？
----------------
sessionid，通过session_id()函数可以取得当前的session_id。


session和cookie的使用步骤分别是什么？什么是sesssion和cookie的生命周期？session和cookie的区别是什么？
-----------------
cookie是保存在客户端机器的，对于未设置过期时间的cookie，cookie值会保存在机器的内存中，只要关闭浏览器则cookie自动消失。如果设置了cookie的过期时间，那么浏览器会把cookie以文本文件的形式保存到硬盘中，当再次打开浏览器时cookie值依然有效。  

session是把用户需要存储的信息保存在服务器端。每个用户的session信息就像是键值对一样存储在服务器端，其中的键就是sessionid，而值就是用户需要存储信息。服务器就是通过sessionid来区分存储的session信息是哪个用户的。  

两者最大的区别就是session存储在服务器端，而cookie是在客户端。session安全性更高，而cookie安全性弱。  

session在web开发中具有非常重要的份量。它可以将用户正确登录后的信息记录到服务器的内存中，当用户以此身份访问网站的管理后台时，无需再次登录即可得到身份确认。而没有正确登录的用户则不分配session空间，即便输入了管理后台的访问地址也不能看到页面内容。通过session确定了用户对页面的操作权限。  

使用session的步骤：
```
1. 启动session：
使用session_start()函数来启动。
2. 注册会话：
直接给$_SESSION数组添加元素即可。
3. 使用会话：
判断session是否为空或者是否已经注册，如果已经存在则像普通数组使用即可。
4. 删除会话：
1.可以使用unset删除单个session；
2.使用$_SESSION=array()的方式，一次注销所有的会话变量；
3.使用session_destroy()函数来彻底销毁session。
```

cookie怎么使用？
1. 记录用户访问的部分信息
2. 在页面间传递变量
3. 将所查看的internet页存储在cookies临时文件夹中，可以提高以后的浏览速度。

创建cookie：  
setcookie(string cookiename , string value , int expire);  

读取cookie：  
通过超级全局数组$_COOKIE来读取浏览器端的cookie的值。  

删除cookie：有两种方法  
1.手工删除方法：  
右击浏览器属性，可以看到删除cookies，执行操作即可将所有cookie文件删除。  
2.setcookie()方法：  
跟设置cookie的方法一样，不过此时将cookie的值设置为空，有效时间为0或小于当前时间戳。  


如何设置一个cookie的名字为username,值为jack，并且让此cookie一周后失效？
--------------
setcookie(‘username’,’jack’,time()+7*24*3600);

一个浏览器最多可以产生多少个cookie，每个cookie文件最大不能超过多少？
--------------
最多可以产生20个cookie，每个最多不超过4K



设置或读取session之前，需要做什么？
--------------
可以直接在php.ini中开启session.auto_start = 1或者在页面头部用session_start();
开启session，session_start()前面不能有任何输出，包括空行。


在实际开发中，session在哪些场合使用？
--------------
session用来存储用户登录信息和用在跨页面传值。  
1）常用在用户登录成功后，将用户登录信息赋值给session；  
2）用在验证码图片生成，当随机码生成后赋值给session。  


注销session会话的形式有几种？
--------------
unset();   
$_SESSION=array();  
session_destroy();  


什么是OOP?什么是类和对象？什么是类属性？
--------------
OOP(object oriented programming)，即面向对象编程，其中两个最重要的概念就是类和对象。
世间万物都具有自身的属性和方法，通过这些属性和方法可以区分出不同的物质。  

属性和方法的集合就形成了类，类是面向对象编程的核心和基础，通过类就将零散的用于实现某个功能的代码有效地管理起来了。

类只是具备了某些功能和属性的抽象模型，而实际应用中需要一个一个实体，也就是需要对类进行实例化，类在实例化之后就是对象。★类是对象的抽象概念，对象是类的实例化。

OOP具有三大特点：1. 封装性（又叫做隐藏性）；2. 继承性； 3. 多态性。

OOP的优点：1、代码重用性高（省代码） 2、使程序的可维护性高（扩展性） 3、灵活性



常用的属性的访问修饰符有哪些？分别代表什么含义？
--------------
private，protected，public。  

访问权限：  
类外：public ,var  
子类中：public，protected ,var  
本类中：private，protected，public ,var  

如果不使用这三个关键词，也可以使用var关键字。但是var不可以跟权限修饰词一起使用。var定义的变量，子类中可以访问到，类外也可以访问到，相当于public。  

类前面：final，abstract  (final的类不能被继承，abstract是抽象类也不能被继承)  
属性前面：必须有访问修饰符（private，protected，public，var）  
方法前面：static，final，private，protected，public ，abstract  


final关键字能定义类中的成员属性吗？
---------------
答：不能，类的成员属性只能有public ，private ， protected ，var 来定义


说说static关键字的使用场合？static能用在class前吗？
static可以跟public，protected，private一起使用吗？构造方法可以是static的吗？
---------------
static可以在属性和方法前面使用，调用static属性或者方法时，只要将类载入就可用，不用实例化  
static不能用在class的前面  
static可以跟public，protected，private一起使用，在方法的前面；  
构造方法不能是static  


class前面能加访问修饰符吗？如果能加，只能是哪几个访问修饰符？可以是权限访问修饰符public，protected，private吗？
---------------
class前面可以加final，abstract；   
class前面不能加public，protected，private  


$this和self、parent这三个关键词分别代表什么？在哪些场合下使用？
-------------
$this代表的是当前对象  
self代表的是当前的类  
parent代表的是当前类的父类  

使用场合：  
$this只能使用在当前类中，通过$this->可以调用当前类中的属性和方法；  
self只能在当前类中使用，通过作用域操作符::访问当前类中的类常量、当前类中的静态属性、当前类中的方法；  
parent只能使用在有父类的当前类中，通过作用域操作符::访问父类中的类常量、父类中的静态属性、父类中的方法。  


作用域操作符在那些场合下使用
-------------
```
a) 本类中：
i. self::类常量
ii. self::静态属性
iii. self::方法() parent::方法()
b) 子类中：
i. parent::类常量
ii. parent::静态属性（public或者protected）
iii. parent::方法()（public或者protected）
c) 类外：
i. 类名::类常量
ii. 类名::静态属性（public）
iii. 类名::静态方法（public）
```


类中如何定义常量、如何类中调用常量、如何在类外调用常量。
--------------
类中的常量也就是成员常量，常量就是不会改变的量，是一个恒值。
定义常量使用关键字const.  
例如：const PI = 3.1415326;  
无论是类内还是类外，常量的访问和变量是不一样的，常量不需要实例化对象，
访问常量的格式都是类名加作用域操作符号（双冒号）来调用。  
即：类名 :: 类常量名;  


作用域操作符::如何使用？都在哪些场合下使用？
--------------
调用类常量  
调用静态变量，静态方法   


什么是魔术方法？常用的魔术方法有哪几个？
--------------
以__开头的系统自定义的方法。

__construct()
__destruct()
__autoload()
__call()
__tostring()



什么是构造方法和析构方法？
---------------
构造方法就是在实例化一个对象的同时自动执行的成员方法，作用就是初始化对象。  
php5之前，一个跟类名完全相同的方法是构造方法，php5之后魔术方法__construct()就是构造方法。
如果类中没有定义构造方法，那么php会自动生成一个，这个自动生成的构造方法没有任何参数，没有任何操作。  
构造方法的格式如下：  
function __construct(){}  
或者：function 类名(){}  
构造方法可以没有参数，也可以有多个参数。  

析构方法的作用和构造方法正好相反，是对象被销毁时被自动调用的，作用是释放内存。  
析构方法的定义方法为：__destruct();  
因为php具有垃圾回收机制，能自动清除不再使用的对象，释放内存，一般情况下可以不手动创建析构方法。  


__autoload()方法的工作原理是什么？
---------------
使用这个魔术函数的基本条件是类文件的文件名要和类的名字保持一致。
当程序执行到实例化某个类的时候，如果在实例化前没有引入这个类文件，那么就自动执行__autoload()函数。  
这个函数会根据实例化的类的名称来查找这个类文件的路径，当判断这个类文件路径下确实存在这个类文件后就执行include或者require来载入该类，然后程序继续执行，如果这个路径下不存在该文件时就提示错误。  
使用自动载入的魔术函数可以不必要写很多个include或者require函数。  



什么是抽象类和接口？抽象类和接口有什么不同和相似的地方？
----------------
接口是帮助php实现功能意义上的多继承的，用interface来声明，其方法没有方法体，使用implemens关键词来实现接口。  
接口中只能包含抽象方法和类常量，不可以包含成员属性。  

抽象类是一种不能被实例化的类，只能作父类，用abstract class来定义，抽象类和普通类可以没有区别，类中可以包含成员属性、类常量、方法。
子类得用extends来继承，而且只能是单继承。  

相同点：  
都不可以被实例化。  

区别：  
接口可以实现多继承，而抽象类只能是单继承。  
接口中不能包含成员属性，而抽象类中可以有成员属性。
接口中的抽象方法必须是public或者无访问修饰词，接口中的抽象方法不能用abstract来修饰。
抽象类中的方法可以是普通方法，也可以是抽象方法，如果是抽象方法，一定需要使用abstract来修饰。  


抽象类至少有一个抽象方法吗？
-----------------
如果一个类声明成抽象类，里面可以没有抽象方法  
如果一个类中有抽象方法，这个类必须是抽象类  


smarty配置主要有哪几项？
-----------------
1. 引入smarty.class.php;  
2. 实例化smarty对象；  
3. 重新修改默认的模板路径；  
4. 重新修改默认的编译后文件的路径；  
5. 重新修改默认的配置文件的路径；  
6. 重新修改默认的cache的路径。  
7. 可以设置是否开启cache。  
8. 可以设置左侧和右侧定界符。  



MVC的概念是什么？各层主要做什么工作？
-----------------
MVC（即模型-视图-控制器）是一种软件设计模式或者说编程思想。
M指Model模型层，V是View视图层（显示层或者用户界面），C是Controller控制器层。
使用mvc的目的是实现M和V分离，从而使得一个程序可以轻松使用不同的用户界面。

在网站开发中，  
模型层一般负责对数据库表信息进行增删改查，  
视图层负责显示页面内容，  
控制器层在M和V之间起到调节作用，控制器层决定调用哪个model类的哪个方法，
执行完毕后由控制器层决定将结果assign到哪个view层。  


方法重载和重写的区别
---------------
方法重写(overriding)：
　　1、也叫子类的方法覆盖父类的方法，要求返回值、方法名和参数都相同。  
　　2、子类抛出的异常不能超过父类相应方法抛出的异常。(子类异常不能超出父类异常)  
　　3、子类方法的的访问级别不能低于父类相应方法的访问级别(子类访问级别不能低于父类访问级别)  
方法重载(overloading):重载是在同一个类中的两个或两个以上的方法，拥有相同的方法名，但是参数却不相同，方法体也不相同，最常见的重载的例子就是类的构造函数，可以参考API帮助文档看看类的构造方法。  

php支持重写，不支持重载。  



__tostring()魔术方法在什么时候被自动执行？ __tostring()魔术方法必须要return返回值吗？
---------------
当echo或者print一个对象时，就是自动触发。而且__tostring()必须要返回一个值



什么是单点入口呢？
---------------
所谓单点入口就是整个应用程序只有一个入口，所有的实现都通过这个入口来转发，
比如说在上面我们就使用index.php作为程序的单点入口，当然这个是可以由你自己任意控制的。  

单点入口有几大好处：  
1）程序的架构更加清晰明了。  
2）一些系统全局处理的变量，类，方法都可以在这里进行处理。比如说你要对数据进行初步的过滤，你要模拟session处理，你要定义一些全局变量，甚至你要注册一些对象或者变量到注册器里面。  


PHP提供了2套正则表达式函数库,分别是哪两套？
---------------
1) PCRE Perl兼容正则表达式 preg_ 为前缀
2) POSIX 便携式的操作系统接口 ereg_ 为前缀


正则表达式的组成？
------------
原子(普通字符，如英文字符)   
元字符(有特殊功用的字符)   
模式修正字符 

一个正则表达式中，至少包含一个原子   


OOP的三大特性是什么？
------------
1. 封装性：  
也称为信息隐藏，就是将一个类的使用和实现分开，只保留部分接口和方法与外部联系，或者说只公开了一些供开发人员使用的方法。  
于是开发人员只需要关注这个类如何使用，而不用去关心其具体的实现过程，这样就能实现MVC分工合作，也能有效避免程序间相互依赖，实现代码模块间松藕合。

2. 继承性：  
就是子类自动继承其父级类中的属性和方法，并可以可以添加新的属性和方法或者对部分属性和方法进行重写。继承增加了代码的可重用性。php只支持单继承，也就是说一个子类只能有一个父类。

3. 多态性：  
子类继承了来自父级类中的属性和方法，并对其中部分方法进行重写。于是多个子类中虽然都具有同一个方法，但是这些子类实例化的对象调用这些相同的方法后却可以获得完全不同的结果，这种技术就是多态性。多态性增强了软件的灵活性。



常用魔术方法的触发时机？
-------------
1）__autoload() ：当程序实例化某个类，而该类没有在当前文件中被引入。此时会触发执行__autoload()。
程序希望通过该方法，自动引入这个类文件。该方法有一个参数，即就是那个忘记引入的类的名称。

__autoload()方法的工作原理：  
当程序执行到实例化某个类的时候，如果在实例化前没有引入这个类文件，那么就自动执行__autoload()函数。这个函数会根据实例化的类的名称来查找这个类文件的路径，当判断这个类文件路径下确实存在这个类文件后，就执行include或者require来载入该类，然后程序继续执行，如果这个路径下不存在该文件时就提示错误。使用自动载入的魔术函数可以不必要写很多个include或者require函数。  

2）__construct() ：这个是魔术构造方法。构造方法是实例化对象的时候自动执行的方法，作用就是初始化对象。该方法可以没有参数，也可以有多个参数。如果有参数，那么new这个对象的时候要记得写上相应的参数。在php5以前，没有魔术构造方法，普通构造方法是一个跟类名同名的方法来实现构造的。如果一个类中既写了魔术构造方法，又定义了普通构造方法。那么php5以上版本中，魔术方法起作用，普通构造方法不起作用。反之，在php5以前版本中，不认识魔术构造方法，只是把该方法当做普通的方法。

3）__destruct() ：这个是魔术析构方法。析构方法的作用和构造方法正好相反，是对象被销毁时被自动调用的，作用是释放内存。析构方法没有参数。

4）__call() ：当程序调用一个不存在或不可见的成员方法时，自动触发执行__call()。它有两个参数，分别是未访问到的方法名称和方法的参数。而第二个参数是数组类型。

5）__get() ：当程序调用一个未定义或不可见的成员属性时，自动触发执行__get()。它有一个参数，表示要调用的属性的名称。

6）__set()：当程序试图写入一个不存在或不可见的成员属性时，PHP就会自动执行__set()。它包含两个参数，分别表示属性名称和属性值。

7）__tostring() ：当程序使用echo或print输出对象时，会自动调用该方法。目的是希望通过该方法将对象转化为字符串，再输出。__tostring() 无参数，但是该方法必须有返回值。

8）__clone() ：当程序clone一个对象的时候，能触发__clone()方法，程序希望通过这个魔术方法实现：不仅仅单纯地克隆对象，还需要克隆出来的对象拥有原来对象的所有属性和方法。


__sleep()、 __wakeup() 在对对象进行串行化的时候调用
如果序列化对象的时候，不写__sleep()方法，则所有的成员属性都会被序列化，而定义了__sleep()方法，则只序列化指定数组中的变量。
因此，如果有非常大的对象而并不需要完全储存下来时此函数也很有用。

使用 __sleep 的目的是关闭对象可能具有的任何数据库连接，提交等待中的数据或进行类似的清除任务。此外，如果有非常大的对象而并不需要完全储存下来时此函数也很有用。 
使用 __wakeup 的目的是重建在序列化中可能丢失的任何数据库连接以及处理其它重新初始化的任务。



解释PHP中单例模式？
---------------
单例模式又叫做单态模式、单元素模式、singleton pattern。  
单例模式可确保一个类只创建一个实例。使用单例模式的类称为单例类。  

在php中单例类必须要有：
一个私有的静态的成员属性$_instance，用来存储被自身实例化后的对象。  
一个公共的静态的方法getInstance（），该方法返回已经存储了实例对象的$_instance。 
私有构造方法防止除自身以外的类来实例化它。  
私有的方法体为空的克隆方法防止该类被克隆。  


什么是SQL注入？
---------------
SQL注入攻击是黑客对数据库进行攻击的常用手段之一。一部分程序员在编写代码的时候，
没有对用户输入数据的合法性进行判断，注入者可以在表单中输入一段数据库查询代码并提交，
程序将提交的信息拼凑生成一个完整sql语句，服务器被欺骗而执行该条恶意的SQL命令。注入者根据程序返回的结果，成功获取一些敏感数据，甚至控制整个服务器，这就是SQL注入。  


$_REQUEST、$_GET、$_POST、$_COOKIE 的关系和区别： 
---------------
1.关系：$_REQUEST包含了$_GET、$_POST、$_COOKIE等的所有内容，是它们的集合体。  
2.通过$_REQUEST获取变量值，PHP页面因为不确定它是哪种传值方式，因此会根据php.ini中的配置来接收值。

php.ini里可以设置，variables_order = “GPC”。其含义是GET,POST,COOKIE。  
所以PHP页面会先从$_GET中获取，再从$_POST中获取，然后从$_COOKIE中获取。新获得的值会覆盖之前获取到的值。

因此从表现形式上看，$_REQUEST最后是获取$_COOKIE中的值，如果$_COOKIE中没有值，
会获取$_POST中的值，如果$_POST没有获取到 ，就去$_GET中获取。
如果$_GET中也没有该值，那么$_REQUEST就返回null。  



文件上传需要注意哪些细节？怎么把文件保存到指定目录？怎么避免上传文件重名问题？
---------------
```
1). 首现要在php.ini中开启文件上传；
2). 在php.ini中有一个允许上传的最大值，默认是2MB。必要的时候可以更改；
3). 上传表单一定要记住在form标签中写上enctype="multipart/form-data"；
4). 提交方式 method 必须是 post；
5). 设定 type="file" 的表单控件，并且必须具有name属性值；
6). 为了上传成功，必须保证上传文件的大小是否超标、文件类型是否符合要求，上传后存放的路径是否存在；
7). 表单提交到接收页面，接收页面使用$_FILES来接收上传的文件。
$_FILES是个多维数组。第一维下标是上传控件的name，二维下标分别为name/type/tmp_name/size/error。分别代表
文件名、文件类型、上传到临时目录下的临时文件名、文件大小、是否有错误。
如果是批量上传，那么二维下标就是数组，而并非是字符串。
8). 文件上传后是被放置在服务器端临时路径下，需要使用move_uploaded_file ()函数，才可以将上传后的
文件保存到指定目录。
9). 为了避免上传文件重名，可以通过上传的文件名获取到文件后缀，然后使用时间戳+文件后缀的方式为文件重新命名。
```


PHP的写时复制机制（Copy-On-Write)
------------
例如这种形式
```
$a = 1;
b = $a; //当把a赋值给b时，在内存中a,b其实是指向同一块内存
$b = 2; //只有当b值发生变化，才会内存复制赋新值
```
写时复制优点：是通过赋值的方式赋值给变量时不会申请新内存来存放新变量所保存的值，而是简单的通过一个计数器来共用内存，只有在其中的一个引用指向变量的值发生变化时才申请新空间来保存值内容以减少对内存的占用。

从PHP底层基础数据结构来看  
ref_count和is_ref是定义于zval结构体中  
is_ref标识是不是用户使用 & 的强制引用；  
ref_count是引用计数，用于标识此zval被多少个变量引用，即写时复制的自动引用，为0时会被销毁  



urlencode和rawurlencode的区别
------------
urlencode和rawurlencode两个方法在处理字母数字，特殊符号，中文的时候结果都是一样的，唯一的不同是对空格的处理，urlencode处理成“+”，rawurlencode处理成“%20”。



在网站开发中需要传递变量值时，不能使用session、cookie、application,你有几种方法
-------------
Session id的传递主要有四个方法：

1、 通过cookie。

2、 设置php.ini中的session.use_trans_sid = 1或者编译时打开打开了--enable-trans-sid选项，让PHP自动跨页传递session id。

3、 手动通过url或隐藏表单传值。

4、 用文件或数据库方式传递，在通过其他key对应取值。

以上的2和3其实使用的是同样的方法，只是途径不一样。

通过以上的分析我们不难看出，通过cookie传递session id，将session存储于memcache服务器中是为一个比较合理的选择。当出现跨域的情况是，可以使用p3p跨域设置cookie。而当客户端禁用cookie的情况下，可以设置php.ini，通过url自动传递session id。


反射
---------------
参考：https://www.cnblogs.com/nixi8/p/5176213.html

反射是在PHP运行状态中，扩展分析PHP程序，导出或提取出关于类、方法、属性、参数等的详细信息，包括注释。这种动态获取的信息以及动态调用对象的方法的功能称为反射API。

反射是操纵面向对象范型中元模型的API，其功能十分强大，可帮助我们构建复杂，可扩展的应用。

其用途如：自动加载插件，自动生成文档，甚至可用来扩充PHP语言。

php反射api由若干类组成，可帮助我们用来访问程序的元数据或者同相关的注释交互。借助反射我们可以获取诸如类实现了那些方法，创建一个类的实例（不同于用new创建），调用一个方法（也不同于常规调用），传递参数，动态调用类的静态方法。

反射api是php内建的oop技术扩展，包括一些类，异常和接口，综合使用他们可用来帮助我们分析其它类，接口，方法，属性，方法和扩展。这些oop扩展被称为反射。

通过ReflectionClass，我们可以得到Person类的以下信息：

1）常量 Contants

2）属性 Property Names

3）方法 Method Names静态

4）属性 Static Properties

5）命名空间 Namespace

6）Person类是否为final或者abstract

垃级回收,gc
-----------
gc默认是开启的
a) 通过php.ini中 zend.enable_gc 来开启或则关闭GC  
b) PHP代码中我们可以通过gc_enable()和gc_disable()函数来开启和关闭GC  

php的垃级回收是借助引用计数。

每个PHP的变量都存在于一个叫做zval的容器中,一个zval容器,除了包含变量名和值,还包括一个is_ref和一个refcount。refcount即引用计数。  

php5.2之前只是通过refcount是否等0来回收内存。每个PHP的变量都存在于一个叫做zval的容器中
“引用计数”存在问题，就是当两个或多个对象互相引用形成环状后，内存对象的计数器则不会消减为0；这时候，这一组内存对象已经没用了，但是不能回收，从而导致内存泄露。  

php5.3开始，使用了新的垃圾回收机制，在引用计数基础上，实现了一种复杂的算法，来检测内存对象中引用环的存在，以避免内存泄露。  

1：如果一个zval的refcount增加，那么此zval还在使用，肯定不是垃圾，不会进入缓冲区。  
2：如果一个zval的refcount减少到0，那么zval会被立即释放掉，不属于GC要处理的垃圾对象，不会进入缓冲区。  
3：如果一个zval的refcount减少之后大于0，那么此zval还不能被释放，此zval可能成为一个垃圾，将其放入缓冲区。当缓冲区达到了临界值，PHP会自动调用一个方法去遍历每一个值，如果发现是垃圾就清理。PHP5.3中的GC针对的就是这种zval进行的处理。  



避免PHP-FPM内存泄漏导致内存耗尽
----------
对于PHP-FPM多进程的模式，想要避免内存泄漏问题很简单,就是要让PHP-CGI在处理一定数量进程后退出即可。
否则PHP程序或第三方模块(如Imagemagick扩展)导致的内存泄漏问题会导致内存耗尽或不足。
php-fpm.conf中有相关配置：
> pm.max_requests = 1024  #请自行按需求配置

实际上还有另一个跟它有关联的值max_children，这个是每次php-fpm会建立多少个进程，这样实际上的内存消耗是max_children*max_requests*每个请求使用内存。

另外一些粗暴的方法包括建立cron kill掉占用内存过多的php-cgi，

检查php进程的内存占用，杀掉内存使用超额的进程

一般情况下，如果php-cgi进程占用超过1%的内存，就得考虑一下是否要杀掉它了。因为普通情况下，php-cgi进程一般占用0.2%或以下。

这里提供一个脚本供各位使用，就是放在cron任务里，每分钟执行一次。

使用crontab -e 命令，然后添加如下调度任务

* * * * * /bin/bash /usr/local/script/kill_php_cgi.sh

kill_php_cgi.sh脚本如下
```
#!/bin/sh
#如果是要杀掉php-fpm的进程，下面的语句中php-cgi请改成php-fpm
pids=`ps -ef|grep php-cgi|grep -v "grep"|grep -v "$0"| awk '{print $2}'`
if [ "$pids" != "" ];then
	for  pid  in   $pids;
	do
		kill -9 $pid
	done
fi
```

