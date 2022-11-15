## Password modification of any user in the background of ofcms

### 1、 Vulnerability introduction

Ofcms is a Java version CMS system, a content management system developed based on Java technology, with functions such as column template customization, content model customization, multiple site management, online template page editing, etc., full open source code, and MIT license agreement.

Technical model selection: jfinal DB+Record mysql freemarker Encache spring, etc.

Features: It supports multiple sites and can add mobile phone stations and PC stations as required.

In the ofcms background system, ordinary users modify the passwords of other users, even super administrator passwords, by modifying the ID value in the password interface.

Vulnerability address: http://localhost:8081/ofcms-admin/admin/index.html

Code download address: https://gitee.com/oufu/ofcms.git

### 2、Vulnerability verification

##### （1）Vulnerability location

Log in to the low privilege account and the high privilege account respectively. The low privilege account can be modified through the password interface. Because the background checks the original password, that is, the user password corresponding to the ID can be modified by tampering with the ID in the data package. Vulnerability address: http://localhost:8081/ofcms-admin/admin/index.html

##### (2) Vulnerability trigger conditions

The password interface can be modified for low privilege accounts. Because the original password is verified in the background, the user password corresponding to the ID can be modified by tampering with the ID in the data package.

##### （3）Vulnerability recurrence

Log in to the super administrator account and create a common user with the user name of "ceshi". The following figure shows the homepage of the super administrator.

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic1.png" alt="pic1" style="zoom:150%;" />

For ordinary users with a login user name of "ceshi", the left navigation bar of ordinary users is empty, and the super administrator function cannot be used. The following figure shows the homepage interface of ordinary users.

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic2.png" alt="pic1" style="zoom:150%;" />

Click Modify Password in the common user interface to intercept the data package, as shown in the following figure.

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic3.png" alt="pic1" style="zoom:150%;" />

Ordinary users can modify the user_ ID value to modify the password of other users, such as super administrator user_ If the ID is "1", we can change it to "1" to change the super administrator password.

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic4.png" alt="pic1" style="zoom:150%;" />

The original password of super administrator "123456" has been changed to "ceshi123"

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic5.png" alt="pic1" style="zoom:150%;" />

### 3、Code audit

Trace the resetpwd.json interface。File Path： ofcms-master/ofcms-admin/src/main/java/com/ofsoft/cms/admin/controller/system/SysUserController.java 113line

<img src="/Users/liuhao/markdown-master/ofcms后台任意用户密码修改/pic/pic6.png" alt="pic1" style="zoom:150%;" />

In the respwd method, the passwords entered twice will be compared first. If they are inconsistent, they will be returned directly. If they are consistent, the passwords will be encrypted by Sha256Hash and then set to the Record object. The Record object encapsulates a Map object, and you can get and set it. The most important thing is that you also get the user_ The id is also set to the Record object. Then execute Db update。 User_ The ID is used as the update condition to obtain the user from the record_ ID, and then the user in record_ The ID is actually the user passed in from outside_ id 。 So you can control the user_ Id to reset any user password.