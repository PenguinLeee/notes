# Shodan把玩心得

李启闻 3120201453

## 0x01 选课相关

我是数学与统计学院的研究生，选课的时候研究生似乎没有资格选本科生的校公选课...但是因为研究方向和网络安全相关还是想跟着上一上: )

## 0x02 第一次尝试

Shodan提供了web页面和多种语言、多种工具的api。我主要把玩了一手Metasploit和Python的客户端/

**Metasploit客户端**

**Python客户端**

Python中有shodan客户端库，首先使用下列命令安装shodan库：

```shell
pip install shodan
```

安装完成之后，撰写脚本shodan_test.py：

```python
# shodan_test.py
import shodan as sh
import sys

if __name__ == "__main__":
	keywords = sys.argv[1]
	SHODAN_API_KEY = "8dTzButSb505OidzAjQJSV9NLRDuceQU"
	api = sh.Shodan(SHODAN_API_KEY)
	try:
		results = api.search(keywords)
		print(f"Results found: {results['total']}")
		for result in results['matches']:
			print(f"IP: {result['ip_str']} \t port: {result['port']} \t OS: {result['os']}" )
	except sh.APIError:
		print("APIError!")
	except:
		print("Unknown Error!")
```

其中，变量`SHODAN_API_KEY`由用户指定，可见于shodan的`My Account`中。

![account](C:\Users\Lenovo\Desktop\account.png)

在cmd中输入以下指令以寻找摄像头：

```shell
# python shodan_test.py [搜索关键字]
python shodan_test.py webcam
```

结果如下：

![扫描](C:\Users\Lenovo\Desktop\扫描.JPG)

然后随便找了一个网站试了一手

![界面](C:\Users\Lenovo\Desktop\界面.JPG)

接着我试图对账号密码进行手动爆破，然而并没有成功。

*注：我曾在2021年3月12日对219.156.240.6:60001进行手动爆破，使用user, passwd='admin', '111111'取得成功，当时显示需要安装flash，试图通过安装flash解决视频播放问题未果，故没有给出截图。2021年3月14日再次访问便无法访问...*

## 0x03 第二次尝试

直接在shodan.io上进行搜索。

先直接使用关键词webcam进行搜索，但是搜索结果中有很多被打上`Honeypot`标签，怀疑是蜜罐所以没敢渗透。随后考虑到打上`Honeypot`标签的IP大多来自于中美欧等强国，因此转变策略，考虑选取一些比较拉跨但是又不是特别拉跨的国家进行实验。以乌克兰、土耳其、墨西哥为关键字进行搜索，下面是乌克兰的webcam搜索结果：

![搜索](C:\Users\Lenovo\Desktop\搜索.JPG)

可以看到直接有图片预览...这个防护措施就约等于没有了。

下面选择了一个带有登录界面的IP（91.206.111.100:8800），使用Burpsuite进行弱密码爆破：

![目标](C:\Users\Lenovo\Desktop\目标.JPG)

先使用Burpsuite对浏览器流量进行代理（设置浏览器和Burpsuite的相应选项即可），然后随便输入user和passwd并试图登录，目的是让Burpsuite收到发送的http报文的模板，在之后的动作中做相应的修改。

发送登录数据的格式如下：可以看到用户名和密码直接明文传输。在爆破时，我尝试过将cookie去掉和保留两种情况，但是http响应在两种情况下都返还给我相同的cookie，可以认为这个网站将第一次上传的登录信息做了相应处理然后赋予cookie。

![image-20210314213305627](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210314213305627.png)

下面在Intruder中对password进行弱口令爆破，其中弱口令集来自于BITNSC。

相关配置如下：首先在已知的模板中选择要替换的文本，即上图中intruder-positions-raw中的username和password；然后设置payload，username只使用admin，password使用外部导入的弱密码集合。![配置](C:\Users\Lenovo\Desktop\配置.JPG)

然后在Burpsuite菜单的Intruder中选择开始攻击。

攻击开始，但是就目前得到的情况来看，似乎并没有成功破解...但是如果有多台计算机的话，可以大幅提高爆破的频率，达成类似于DDoS攻击的效果。由于担心承担法律责任，我没有将Burpsuite设置成DDoS模式。

![攻击中](C:\Users\Lenovo\Desktop\攻击中.JPG)

## 0x04 总结与思考

1. 虽然现在人们对网络安全越来越重视，但是网络上还是有大量使用弱口令的易受击设备。一旦受击，可以造成相关的隐私泄露或者其它危害；
2. 数据传输尽量使用https而不是http。同时，客户端向服务端发送密码等敏感信息时最好发送散列后的值，以防窃听攻击或者中间人攻击；
3. 成功发起一次网络攻击的代价将越来越高，表现为非弱口令设备比例增多（半年前曾经对shodan进行过粗浅的把玩，当时仍有大量弱密码/无密码设备）。
4. 人生苦短，我用Python