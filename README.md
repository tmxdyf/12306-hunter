12306-hunter
============

Java Swing C/S版本12306订票助手

> **本程序完全开放源代码，仅作为技术学习研究交流之用，不得用于任何商业用途；作者不承担任何由此带来的直接或间接责任**


* **特别说明**： 整个程序除了速度和效率高一些外，和浏览器订票请求没有本质区别，因此如果12306服务器做了任何调整，程序随时可能失效，请自行酌情使用。

* **强烈建议**： “不要把所有鸡蛋放到一个篮子”，可注册多个账号，一些用于浏览器插件或常规订票，一些用此程序刷票，这样相对更保险。

### 项目说明

基于HttpClient、Multiple Thread、File I/O等主要技术的Java Swing桌面应用，至于说用途就不多说了，你懂的；
虽然说功能上没有办法和目前类似主流的浏览器插件相提并论，但是由于采用直接的HTTP请求模式，我相信效率上一定会更高。 ** 天下武功,唯快不破 **

* 直接以HTTP GET/POST发起最小数量必须的订票请求，相比浏览器插件方式更加快速高效；

* 基于多线程多账号登录并发刷票，更高的订票成功率；

* 基于文件记录最后输入的订票数据，提高交互友好体验；

* 该程序只核心关注以最高效快速提交订票请求，不支持诸如自动登录、识别验证码、支付等其他高级功能！

整个程序参考了一个名为mygod-go-home的项目，其中还包括一些自动化识别验证码的尝试，在此对于作者的开源共享表示感谢，但是不知道什么原因目前已经很久没有更新发布了。
原来程序可能是考虑太多太全整个代码结构看起来比较费劲，把其中一些请求参数定义组装和响应解析等体力代码引用过来，然后加入自己的想法设计从而有了这个程序工程。

题外话： 对于这样每到逢年过节炒的火热的订票助手，各大浏览器的插件以及12306之间的恩怨纠葛、道德讨论等我觉得已经够多了，我只能说这就是一个在无聊以及无奈的环境下的产物。
我们还是以技术的角度去看待它，自从有了12306.cn，作为标准程序员闲的没事就有了新的乐趣练练手，既然拥有这样的技术，并且能让技能为我所用，何乐而不为呢。说不定哪天就被XXX封杀潜规则了，谁知道呢，权当娱乐而已！

### 用法说明

程序采用Java语言编写实现，因此需要安装Java运行环境。理论上Java 5,6,7 版本皆可运行。

* 直接运行程序：

如果系统已安装过Java运行环境，则直接执行startup.bat即可。

当然如果不懂Java也没关系，请自行访问Oracle下载安装Java运行环境：

http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html

选择“Accept License Agreement”，然后点击相应系统版本下载安装即可（可能需要重启系统），之后执行startup.bat即可。

* 开发模式运行：

本项目本身是一个完整的Eclipse工程，基本Maven依赖管理，熟悉对应开发过程及有兴趣开发调试程序的，可在导入开发工具，以Java应用程序方式执行TicketMainFrame即可。

Swing界面开发采用Eclipse WindowBuilder自动生成框架代码，可自行安装插件以可视化模式查看设计实现。

### 界面截图

![Index View](https://raw.github.com/xautlx/12306-hunter/master/snapshot/index.gif)

![Submit View](https://raw.github.com/xautlx/12306-hunter/master/snapshot/submit.gif)

### 功能说明


> 如果有任何问题或建议反馈，请到 https://github.com/xautlx/12306-hunter/issues 提交Issue；

> 对于程序本身的不足或下面提到的优化点，欢迎有兴趣的朋友本着交流学习为目的的代码改进优化并直接提交Pull Request。

参考上述截图，对于UI界面功能从上至下大致说明如下：

目前程序除了基于乘车起始站对车次做基本校验外，其余基本没有更仔细严格的校验，使用时请自行按照官方网站给的有效数据格式填写，也欢迎补充提交完善校验逻辑代码共同完善程序。

TODD：考虑加入配置文件概念，如可以定制化请求间隔时间（目前代码层面固定的0.5秒）等

* **数据记住功能：** 程序在关闭时自动记录最后输入的相关数据免去下次打开程序重复输入，不包括数据：密码、验证码、乘车日期（每次打开始终自动初始化为20天预售期）、 其他动态日志等信息

* **起站、到站：** 请输入精确的乘车站名称，如北京西（TODO：支持中文或拼音输入提示）

* **乘车日期：** 每次打开始终自动初始化为20天预售期，可自行修改为预售期内的有效日期，请保持默认的日期格式（TODO：日历组件输入支持；校验输入日期在预售期内）

* **备选日期：** 主要用于刷“退票”的时候，碰到他人退票自动快速下单，按照顺序优先级填入逗号分隔的乘车“日”字符串，程序自动换算日期属于本月还是下月；
                                         如当前是10号，填写2,1,29,28则表示按照下月2号，1号，本月29号，28号的顺序不断循环尝试订票，直到其中任何一次成功
                                         
* **用户及车次设置：** 为了提高成功率，可以添加多个注册的12306账号（点击每个行项前面的加减号），每个登录账号各启动一个线程并发订票，各登录账号可根据所需指定相同订票车次或不同车次组合。

 每个登录账号可从【左至右优先级】设定5个【车次和席别】（一个车次可以以不同席别添加多次） ， 程序订票规原则是尽量先定优先级高的票，实在没票了才委屈求全定后续优先级低的票； 简单说就是程序不是按照优先级一个个顺序循环尝试订票，而是始终先不断尝试订优先级高的票，直到系统返回已经没票了才会转入下一车次席别。                   

 因此请合理设置各账号车次席别顺序和组合方式，因为各登录账号订票线程互相独立运行，各自都随时有可能按照上述的订票原则订到指定优先级的车次和席别的票，设定不合理就会导致优先抢到“不抢手”的票了，再想回头想抢中意的票估计就来不及了。
当然也不用考虑太复杂，每个登录账号和车次席别自动发现有票时，会自动弹出下单验证码输入对话框，如果感觉不是自己中意的票可以点击取消即可从而自动再继续尝试刷票，避免不必要的误伤了。

* **账号、密码、验证码：** 这几个什么好说的，顺序输入即可，验证码会自动转为大写并且在满4位后自动触发点击登录请求。验证码图片看不清可以点击刷新。下方是每个登录用户的Cookie数据，显示参考不用太关注。
（TODO：考虑加一个登录状态的守护线程，防止由于登录后长时间没有发起请求导致登录失效）

* **车次、席别：** 一个登录账号可以分别输入多个车次（包括字面前缀的完整车次）及对应席别，从左到右优先级，规则见上述说明 ；具体车次代码和有效的席别请自行通过12306网站查询。（TODO：可考虑加入车次对应席别有效性的校验）

* **乘车人：** 没啥好说的，按照网站类似的填写相关信息即可，可点击加减号增减多个乘车人，建议从12036常用联系人拷贝相关数据，避免手工输入错误（TODO：添加从12306获取乘车人信息及有效性校验）

* **开始自动刷票：** 基于“已成功登录的账号”和“已勾选的乘车人”，及相关填写信息启动刷票线程，期间刷到票后后自动弹出顶层窗口显示相关车次信息和输入验证码，确认是需要的票的赶快输入验证码（输入4位自动提交）提交下单，如果验证码错误会再次弹出窗口输入；如果不是想要的车票则点击取消即可。
任何一个账号线程提示订票成功后会当前线程自动终止，但是其他账号线程还会继续，可以点击“停止自动刷票”结束所有刷票线程。  

* **停止自动刷票：** 强制结束所有刷票线程。 

