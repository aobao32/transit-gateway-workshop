# Transit Gateway Workshop 所用之系列 CloudFormation Template

在本次AWS网络专题培训中，讲解了如下内容：

- 1、VPC基础知识
- 2、创建VPC和EC2的操作演示
- 3、创建两个VPC之间的Peering打通VPC
- 4、一个Region内多个VPC之间使用Transit Gateway组建星形网络
- 5、跨Region多个VPC之间使用Inter-region Peering组建网络

在其中第3、4、5课中，为了快速准备实验环境，需要使用 CloudFormation，一键部署VPC环境。

# 使用方式

使用方法如下：

- 1、使用git整体Checkout出来，将文件cloudformation目录中的文件拿到PC机本地；或者直接在网页上查看文件内容，复制到剪切板中，然后粘贴到文本编辑器中。两种方法二选一。
- 2、将保存在本地的文件，上传到要操作Cloudformation的AWS账号对应的S3的一个bucket中。注意操作环境需要匹配。如果在AWS中国区实验，则必须使用宁夏和北京Region的S3 Bucket。如果在AWS Global实验，则S3必须使用Global S3 Bucket。
- 3、进入Cloudformation模块，选择要使用的模版位于S3上，粘贴上S3的地址，即可开始后续操作。
