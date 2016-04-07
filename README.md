### 项目已转移到Github:https://github.com/tolerious/django-wechat
### 微信企业号Django版本

请根据需要修改wechat.ini中的内容,一般是可以选择单跑uwsgi或者是nginx+uwsgi滴,当然，你也可以另类些，只跑django，LOL.....uwsgi跟nginx的相关配置可以参考`uwsgi+nginx`配置一节

### uwsgi+nginx配置
待补充，懒，2333333333

### 配置回调URL及密钥
由于corpid跟corpsecret是很私密的东西，所以不能直接写在源码里面的，我放在了数据库中，所以在使用之前要先进入`python manage.py shell`的环境中去，把你的微信平台对应的信息创建出来.当然了是可以通过在Django后台的那个管理界面上创建register一个界面来实现，懒，
具体：
```
from app.models import *
account = AccessToken()
account.corpid=your_corpid
account.corpsecret=your_corpsecret
account.token = your_token
account.aeskey = your_EncodingAESKey
account.save()
```
当然你也可以通过读写一个文件的形式来操作，根据个人喜好
然后进入到填写回调模式验证的界面，如图:![Alt text](http://ww2.sinaimg.cn/mw690/a036a21agw1eqbat7mbi9j20xg0oqgp5.jpg)
URL处填写你的server IP，后面跟一个validate/，这里填写的token要跟你在上图中所填写的一样，同样aeskey也要跟你在界面上填写的保持一致，这些数据是后续步骤所需要的。
友情提示：如果你在验证过一次后对你的token或者EncodingAESKey做了修改，请重启uwsgi，理论上不需要重启，但是遇到过几次要重启的情况。

### 获取access token的方法

调用`app`模块中AccessToken类中的`get_access_token`方法即可返回access_token

###