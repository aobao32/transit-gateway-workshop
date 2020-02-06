# Transit Gateway Workshop - CloudFormation 模版https://github.com/aobao32/transit-gateway-workshop/blob/master/README.md

在本次AWS网络专题培训中，讲解了如下内容：

- 一、VPC基础知识
- 二、创建VPC和EC2的操作演示
- 三、创建两个VPC之间的Peering打通VPC
- 四、一个Region内多个VPC之间使用Transit Gateway组建星形网络
- 五、跨Region多个VPC之间使用Inter-region Peering组建网络

在其中第三、四、五课中，为了快速准备实验环境，需要使用 CloudFormation，一键部署VPC环境。

# 使用方式

使用方法如下：

- 1、使用git整体Checkout出来，将文件cloudformation目录中的文件拿到PC机本地；或者直接在网页上查看文件内容，复制到剪切板中，然后粘贴到文本编辑器中。两种方法二选一。
- 2、将保存在本地的文件，上传到要操作Cloudformation的AWS账号对应的S3的一个bucket中。注意操作环境需要匹配。如果在AWS中国区实验，则必须使用宁夏和北京Region的S3 Bucket。如果在AWS Global实验，则S3必须使用Global S3 Bucket。
- 3、进入Cloudformation模块，选择要使用的模版位于S3上，粘贴上S3的地址，即可开始后续操作。
- 4、所有Cloudforatmion都要求提前在要使用的区域提前生成EC2登录密钥。

# 模版内容说明

### 实验三、创建两个VPC之间的Peering打通VPC的模版说明

本实验模版如下（支持中国区域）：

- 01-create-1-vpc-1.yml
- 01-create-1-vpc-2.yml

每个模版都会生成VPC、子网、互联网网关、路由表、安全规则组等，且包含EC2实例和PublicIP用于登录。第一个模版使用网段10.1.0.0/16，第二个模版使用网段192.168.0.0/16。然后，即可开始实验，手工创建VPC Peering。

***

### 实验四、一个Region内多个VPC之间使用Transit Gateway组建星形网络的模版说明

本实验使用模版如下（支持中国区域）：

- 02-create-4-vpc.yml 模版，主嵌套调用如下4个模版
  * Nested-Stack-VPC1.yml
  * Nested-Stack-VPC2.yml
  * Nested-Stack-VPC3.yml
  * Nested-Stack-VPC4.yml
  
嵌套模版的使用方法是：将所有模版都上传到S3同一个bucket下。打开文本编辑器修改主模版文件，找到其中调用嵌套模版的几个调用URL网址，将模版内的URL替换为当前所在S3 bucket发布出来的正确网址。然后启动Cloudformation界面，将S3上主模版发布出来的HTTP URL填写到cloudformation界面中即可。

每个模版都会生成VPC、子网、互联网网关、路由表、安全规则组等，且包含EC2实例和PublicIP用于登录。4个VPC模版分别使用网段10.1.0.0/16到10.4.0.0/16。然后，即可开始实验，手工创建Transit Gateway将四个VPC连接起来。

***

### 实验五、跨Region多个VPC之间使用Inter-region Peering组建网络的模版说明

本实验的功能在2019年re-invent2019上发布，目前只在部分Global Region上线，建议在欧洲中部法兰克福（eu-west-1）运行第一组模版，在美国西部俄勒冈（us-west-2）运行第二组模版。AWS中国区暂时不支持。

- 03-create-4-vpc-with-TGW-mesh-route-1 主模版，嵌套调用如下6个模版
  * Nested-Stack-VPC1.yml
  * Nested-Stack-VPC2.yml
  * Nested-Stack-VPC3.yml
  * Nested-Stack-VPC4.yml
  * Nested-Stack-Transit-Gateway-1.yml
  * Nested-Stack-VPC-Route-Table-1.yml

- 03-create-4-vpc-with-TGW-mesh-route-2 主模版，嵌套调用如下6个模版
  * Nested-Stack-VPC5.yml
  * Nested-Stack-VPC6.yml
  * Nested-Stack-VPC7.yml
  * Nested-Stack-VPC8.yml
  * Nested-Stack-Transit-Gateway-2.yml
  * Nested-Stack-VPC-Route-Table-2.yml

嵌套模版的使用方法是：将所有模版都上传到S3同一个bucket下。打开文本编辑器修改主模版文件，找到其中调用嵌套模版的几个调用URL网址，将模版内的URL替换为当前所在S3 bucket发布出来的正确网址。然后启动Cloudformation界面，将S3上主模版发布出来的HTTP URL填写到cloudformation界面中即可。

每个模版都会生成VPC、子网、互联网网关、路由表、安全规则组等，且包含EC2实例和PublicIP用于登录。其中8个VPC模版分别使用网段10.1.0.0/16到10.8.0.0/16。

模版分两个区域分别运行，第1个区域运行第一个模版，将生成VPC1-4，并包含Transit Gateway 1，且VPC1～4已经是通过Transite Gateway 1处于Full-mesh互通状态。第2个区域运行第二个模版，将生成VPC5-8，并包含Transit Gateway 2，且VPC1～4已经是通过Transite Gateway 2处于Full-mesh互通状态。随后，即可开始实验，手工创建Transit Gateway 1～2 之间的 Inter-region Peering 。

# 反馈

请联系 pcman.biti-at-gmail.com
