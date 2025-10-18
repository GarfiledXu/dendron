---
id: 2fy422s42ohwr3e0lfk521f
title: Websit
desc: ''
updated: 1755655444131
created: 1755652720391
---

## Websit

项目地址
印得安运营后台 node 14
https://git.libawall.com/libawall/yda/admin-yda-operation测试：https://testope ration.yda.cn/
前端可在运营后台内自助添加，如没有初始账号可向后台寻求帮助
正式：https://operation.yda.cn/
● 正式环境账号需联系测试
演示：http://operationpre.libawall.com/  （旧）
● 测试环境同号 （留给数据大屏使用）
演示：https://preoperation.libawall.com/ （新）
● 测试环境同号（新pre环境）
备用：http://uatoperation.libawall.com/
● 测试环境同号
UTE：http://uteoperation.libawall.com
● 测试环境同号 
ITE：http://iteoperation.libawall.com
● 测试环境同号 



Jenkins对应流水线
正式——印得安运营后台pro
演示(旧)——印得安运营后台pre
演示(新)——印得安运营后台pre(现)
测试——印得安运营后台test
备用——印得安运营后台uat
ute——印得安运营后台UTE
ite——印得安运营后台ITE
注明： 印得安运营后台test钉钉——已弃用


印得安PC管理端 node 14
https://git.libawall.com/libawall/yda/yda_web_manage企业版
● 测试：https://testydaadmin.libawall.com/
● 正式：https://admin.yda.cn/
● 演示：https://preydaadmin.libawall.com/
● 备用：https://uatydaadmin.libawall.com
● UTE：https://uteydaadmin.libawall.com
● ITE：https://iteydaadmin.libawall.com
政务版——已弃用
● 测试：http://govtestydaadmin.libawall.com/
● 正式：https://admingov.yda.cn/
● 账号密码同印得安APP
（需要去运营后台开通）

Jenkins对应流水线
正式——印得安App管理后台-正式
演示——印得安App管理后台-演示
测试——印得安App管理后台-测试
备用——印得安App管理后台-备用
UTE——印得安App管理后台-UTE
ITE——印得安App管理后台-ITE


印得安小程序

体验版


企业微信（H5）node 16
https://git.libawall.com/libawall/yda/yda-qywx-h5测试：https://testqywx.libawall.com/
正式：https://qywx.yda.cn/
演示：https://preqywx.libawall.com/
UTE：https://uteqywx.libawall.com
ITE：https://iteqywx.libawall.com
调试流程文档：https://b3iqpm8znv.feishu.cn/docx/UEwBdPFz9orOTfxlGX4ch2ENnDd
Jenkins对应流水线
正式——印得安企业微信-正式
演示——印得安企业微信-演示
测试——印得安企业微信-测试
备用——印得安企业微信-备用
UTE——印得安企业微信-UTE
ITE——印得安企业微信-ITE

印得安电子签章 node 16
电子签章用户端
https://git.libawall.com/libawall/yda/yda-electronic-client-admin测试：https://testecc.libawall.com/
正式：https://electronic.yda.cn/
● 登录即注册

Jenkins对应流水线
正式——印得安电子签章-用户端-正式
测试——印得安电子签章-用户端-测试



电子签章运营后台
https://git.libawall.com/libawall/yda/yda-electronic-admin测试：https://testeccadmin.libawall.com/
● 13012345678
正式：https://electronicadmin.yda.cn/

Jenkins对应流水线
正式——印得安电子签章-运营后台-正式
测试——印得安电子签章-运营后台-测试


自助申请H5（企业类型-政府机构） node 16
https://git.libawall.com/libawall/yda/libawall_yda_country_h5id为管理端中的部门id
先打开草料，生成二维码，再用微信扫码
草料二维码：https://cli.im/url/
调试限制：在index.html入口文件内有对微信环境进行判断限制

企业版
测试：https://testyda-h5-country.libawall.com?id=
正式：https://h5-country.yda.cn?id=
演示：https://preyda-h5-country.libawall.com?id=
政务版——已弃用
测试：https://govtestyda-h5-country.libawall.com?id=
正式：https://h5-countrygov.yda.cn?id=

Jenkins对应流水线
正式——印得安H5自助申请-正式
演示——印得安H5自助申请-演示（备用uat环境通用一条流水线，需修改部署命令）
测试——印得安H5自助申请-测试

印得安官网 node 16
https://git.libawall.com/libawall/yda/yda-web-org-new测试：https://testydawww.libawall.com/
正式：https://yda.cn/
正式新：https://www.xidean.com/

Jenkins对应流水线
正式——印得安官网（正式）
测试——印得安官网（测试）

官网后台 node 12/14（有更新要同步给市场运营@郑雯妤 ）
https://git.libawall.com/website/web-ydan-org-manage测试：http://testydawwwadmin.libawall.com/
正式：https://yda.cn/admin/

账号：admin123456
密码：123123123

Jenkins对应流水线
正式——印得安官网后台（正式）
测试——印得安官网后台（测试）



印的安私有化数据看板
git地址：https://git.libawall.com/libawall/yda/yda-private-big-screen.git
印的安App  Flutter2.2.3
git地址：https://git.libawall.com/xuzhengdao/yda-client-new.git
印章柜内嵌App Flutter3.13.0
git地址：https://git.libawall.com/libawall/yda/yda-cabinet-app.git
印控仪内嵌App Flutter2.10.0
git地址：https://git.libawall.com/libawall/yda/hight-camera-app.git

开发资料
Jenkins
https://b.libawall.com/
● yda
● libawall123.
禅道
负责人：@张新海 
https://cd.libawall.com/
● 姓名拼音@yda.cn
● libawall123.

GitLab
负责人：@张新海

蓝湖

## ap

### ute
leitao
15057160296
Xda123456

### test

18888888888
yindean123.

### operation

15049302752
QIANQUAN12l