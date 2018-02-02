# encodeURIComponent-encodeURI
URI编解码详解encodeURIComponent()与encodeURI()


   URI的初识
         若干部分

         1: 协议/scheme name
          类似 https、http、file、ed2k

         2: 主机名
          这部分比较熟悉的是 //baidu.com 网络主机,也可能是其他的形式的资源：mailto:名称@域名 用户邮箱，

          3:query/参数
          类似a=1&b=2

         4: 标识符/锚点
          有的URI指向的不是整个资源，而是某个资源的内部某个模块，
          这也是我们熟悉的锚点：https://baidu.com#test，这是指向资源内部的某一部分。

           5: 相对URI
          有的时候请求的资源可能是相对当前资源的路径来完成的，
          <img src="../icons/logo.gif" alt="logo"> 这样的，

          整体的书写方式如下：  

          <scheme name> : <hierarchical part> [ ? <query> ] [ # <fragment> ]

         例如：

          http://write.blog.csdn.NET/po...

          file:///c:/WINDOWS/clock.avi

          Git://github.com/user/project-name.git

          ftp://user1:1234@地址 
          ed2k://|file|%5BMAC%E7%89%88%E6%9E%81%E5%93%81%E9%A3%9E%E8%BD%A69
          %EF%BC%9A%E6%9C%80%E9%AB%98%E9%80%9A%E7%BC%89%5D.%5BMACGAME%5DNee
          d.For.Speed.Most.Wanted.dmg|4096933888|2c55f0ad2cb7f6b296db9
          4090b63e88e|h=ltcxuvnp24ufx25h2x7ugfaxfchjkwxa|/

          这些都是一个URI Scheme。


         其中：
          <scheme name>：很明显，这是scheme的名称，对于上面五个scheme，
          它们scheme名分别是http,file, git, ftp, ed2k（电驴协议），
          实际上，它们也代表着协议名称。
          
          <hierarchical part>：实际上，一般情况，
          它包含 authority 和 path【//baidu.com、///c:/WINDOWS/clock.avi】。 
          
          <query>：可选项目，一般使用；隔开或&隔开的键值对<key>=<value>【a=1&b=2】
          <fragmentg> ：可选项目包，其它额外的标识信息[#href#anchor]


          和URL的不同:
          这URI和URL长得也很相似，URL是Uniform Resource Locator的缩写，译为“统一资源定位符”。
          格式类似

          协议【http、https】://主机名【baidu.com】/具体地址【test/test.html】【可能还有参数】


            这里有三个概念：

            *URI ：Uniform Resource Identifier，统一资源标识符；

            *URL：Uniform Resource Locator，统一资源定位符；

            *URN：Uniform Resource Name，统一资源名称。是URL的一种更新形式，
            统一资源名称(URN,，Uniform Resource Name)不依赖于位置，
            并且有可能减少失效连接的个数。但是其流行还需假以时日，
            因为它需要更精密软件的支持。

         其中，URL,URN是URI的子集。


URI编解码

            uri中会遇到的两个问题：

          1:在URI经常会出现一些明文内容，例如 https://baidu.com?query=破碎&t=知乎, 
            这样的uri的内容大家都是可见的，这就需要把一些特殊字符进行编码，

            方法：encodeURI() ，把uri进行编码，但是并不会对uri中具有特殊含义的的字符进行编码，
            具体不会编码的部分包括括号中的字符【, / ? : @ & = + $ #】，
             encodeURI('my test.asp?name=ståle&car=saab')
            // my%20test.php?name=st%C3%A5le&car=saab
          
            解码的话可以使用 decodeURI() 
            decodeURI('my%20test.php?name=st%C3%A5le&car=saab') 
            // my test.asp?name=ståle&car=saab


           2: 有的时候uri的某些组成部分自身含有一些特殊字符，这些特殊字符在uri自身在有着特殊意义，
            这样会导致错误的解析uri，例如：

            test.asp?name=sta&le 
            //这里 query的name的值sta&le，包含了页数字符&，是的解析name的值为 sta 就停止了
            
            
            这个时候也需要进行处理。同样也是对进行编码操作，
            方法：encodeURIComponent（），这回对除了字母、数字、(、)、.、!、~、*、'、-和_之外的所有字符，
            而类似【 ：;/?:@&=+$,#】 这些用于分隔 URI 组件的标点符号都会由一个
            或多个十六进制的转义序列替换的。
            
            <scheme name> : <hierarchical part> [ ? <query> ] [ # <fragment> ]
            //上面的描述是一个uri的完整组成，每个部分都可以认为是uri的组件，可以进行单独的编码
            //用户输入的数据作为query参数，用户输入了'test&test=测试'这样的字符串时，只是一个查询参数
            
            var uri = '//test.com';
            var queryValue = 'test&test=测试'; //需要编码，不然会被当成两个键值对了
            var uri += '?query=' + encodeURIComponent(queryValue);
            //这样就是 query = 'test&test=测试'；真正的结果，
            //而不是 query = 'test'; test= '测试' 这样的两个键值对
            
            解码方法：decodeURIComponent()
            
            
