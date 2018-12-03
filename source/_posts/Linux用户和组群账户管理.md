---
title: Linux用户和组群账户管理
date: 2018-11-08 23:22:59
tags: 课程学习
categories: Linux
---

# Linux用户和组群账户管理

在Linux系统中，用户账户是登录系统的唯一凭证，其中root用户是系统的最高管理者，该用户的UID是0级，与用户和组账户相关的配置文件有/etc/passwd、/etc/shadow、/etc/group和/etc/gshadow。  
在Linux系统中可以使用chage命令管理用户口令的时效，防止用户口令由于长时间使用而导致泄露。

## 用户账户管理

### 用户账户概述

用户在Linux系统中是分角色的，由于角色不同，每个用户的权限和所能完成的操作任务也不同。而在实际的管理工作中，用户的角色是通过UID（用户ID号）来标识的，每个用户的UID都是不同的。  
在Linux系统中主要有root用户、虚拟用户和普通用户这三类用户。

#### 1. root用户

root用户是Linux系统的内置用户，在系统中的权限最高，普通用户无法执行的操作，root用户都可以操作，所以也被称之为超级管理用户。在系统中的每个文件、目录和进程都归属于某一个用户，没有用户许可，除root用户外的其他普通用户无法进行操作。  

#### 2. 虚拟用户

这类用户也被称为伪用户或假用户，这类用户不具有登录系统的能力，但却是系统运行不可缺少的用户，比如bin，daemon，adm，ftp以及mail等用户账户，这类用户都是Linux系统的内置用户。  

#### 3. 普通用户

这类用户是由系统管理员创建，并且能登录Linux系统。只能操作自己目录内的文件，权限有限。  

### Linux用户账户配置文件

谈到用户，就不得不谈用户管理、用户配置文件以及用户查询和管理的控制工具。用户管理主要是通过修改用户配置文件完成的，使用用户管理控制工具的最终目的也是为了修改用户配置文件。  

#### 1. /etc/passwd文件

/etc/passwd是系统识别用户的一个文件，Linux系统中所有的用户都记录在该文件中。  

/etc/passwd文件各字段的含义：  
zhangsan:x:1000:1000:张三:/home/zhangsan:/bin/bash  

|字段|含义|
|---|---|
|用户名|也称为登录名，在系统内用户名应该具有唯一性。|
|口令|存放加密的口令，在本例中看到的是一个x，其实口令已经被映射到/etc/shadow文件中了|
|用户标识号|在系统内用一个整数标识用户ID号，每个用户的UID都是唯一的，root用户的UID是0，普通用户的UID默认从1000开始，本例中的用户zhangsan的UID是1000|
|组群标识号|在系统内的一个整数标识用户所属的组群的ID号，每个组群的GID都是唯一的|
|用户名全称|用户名描述，可以不设置。在本例中，zhangsan用户的用户名全称是“张三”|
|主目录|用户登录系统后首先进入的目录，zhangsan用户的主目录是/home/zhangsan|
|登录Shell|用户使用的Shell类型，Fedora 17系统默认使用的Shell是bash|

UID是用户的ID值，在系统中每个用户的UID值是唯一的，更确切地说每个用户都要对应一个唯一的UID。Linux系统用户的UID值是一个正整数，初始值从0开始，在Fedora 17系统中的最大默认值是60000。在Linux系统中，root的UID是0，拥有系统最高权限。UID的唯一性关系到系统的安全，比如在/etc/passwd文件中把用zhangsan的UID改为0后，zhangsan这个用户会被确认为root用户，当用这个账户登录到系统后，可以进行所有root用户才能执行的操作。UID是确认用户权限的标识，用户登录系统所处的角色是通过UID来实现的，而不是用户名。

#### 2. /etc/shadow文件

/etc/shadow文件是/etc/passwd文件的影子文件，这个文件并不是由/etc/passwd文件产生，这两个文件是对应互补的。/etc/shadow文件内容包括用户及被加密的口令及其他/etc/passwd不能包括的信息，比如用户账户的有效期限等。/etc/shadow文件的内容包括9个段位，每个段位之间用“:”分隔。  
zhangsan:$6$E/xvWMmh$rhYLQwwffEqIudVLFzMlvkb0iN4.0 Oluk6H.UovEYN0/99dVoHXcaCNGZZkFY1S3QHYgm7e6JPzEew6 ybmN4e0:16364:0:99999:7:::

/etc/shadow文件各字段的含义

|字段|含义|
|---|---|
|用户名|这里的用户名和/etc/passwd中的用户名是相同的|
|加密口令|口令已经加密，如果有些用户在这里显示的是“!!”,则表示这个用户还没有设置口令，不能登录到系统|
|用户最后一次更改口令的日期|从1970年1月1日算起到最后一次修改口令的时间间隔（天数）|
|口令允许更换前的天数|如果设置为0，则禁用此功能。该字段是指用户可以更改口令的天数|
|口令需要更换的天数|如果设置为0，则禁用此功能。该字段是指用户必须更改口令的天数|
|口令更换前警告的天数|用户登录系统后，系统登录程序提醒用户口令将要过期|
|账户被取消激活前的天数|表示用户口令过期多少天后，系统会禁用此用户，也就是说系统会不让此用户登录，也不会提示用户过期，是完全禁用的|
|用户账户过期日期|指定用户账户禁用的天数（从1970年的1月1日开始到账户被禁用的天数），如果这个字段的值为空，账户永久可用|
|保留字段|目前为空，以备将来Linux系统发展时用|

### 字符界面下用户账户的设置

在Linux系统字符界面下创建、修改以及删除用户账户主要使用useradd，usermod和userdel这三个命令。

#### 创建用户账户

新创建的用户账户默认是被锁定的，无法使用，需要使用passwd命令设置密码以后才能使用。  

使用useradd命令可以在Linux系统下创建用户账户。
命令语法：  
useradd [-u uid [ -o ]] [-g 组群名 ] [ -G 组群名，... ] [ -d home ] [ -s shell ] [-c comment] [ -m [-k template] ] [ -f inactive ] [ -e expire ] [ -p passwd ] [ -M ] [ -n ] [ -r ] [ -l ] [ 用户名 ]  

useradd -D [ -g 组群名 ] [ -b base ] [ -s shell ] [ -f inactive ] [ -e expire ]

> // 创建用户账户zhangsan并设置口令  
useradd zhangsan  
cat /etc/passwd|grep zhangsan  
// 查看/etc/passwd文件，可以看到已经创建了用户zhangsan  
passwd zhangsan  
// 创建用户moon，并设置该用户UID为1510  
useradd -u 1510 moon  
// 创建用户newuser，并设置该用户主目录为/home/www  
useradd -d /home/www newuser  
// 创建用户pp，并指定该用户是属于组群root的成员  
useradd -g root pp  
// 创建用户abc，并设置该用户的Shell类型是/bin/ksh  
useradd -s /bin/ksh abc  

#### 修改用户账户

使用usermod命令能更改用户的Shell类型、所属的用户组群、用户口令的有效期，还能更改用户的登录名。  
命令语法：  
usermod [ -u uid [ -o ] ] [ -g 组群名 ] [ -G 组群名,... ] [ -d 主目录 [ -m ] ] [ -s shell ] [ -c 注释 ] [ -l 新登录名 ] [ -f 失效日 ] [ -e 过期日 ] [ -p 密码 ] [ -L|-U ] [ 用户名 ]

> // 修改用户zhangsan的主目录为/home/kkk，并手动创建/home/kkk目录  
usermod -d /home/kkk zhangsan  
// 修改用户wangwu的主目录为/home/opop，并自动创建/home/opop目录  
usermod -d /home/opop -m wangwu  
// 修改用户wangwu的登录名为zhaoliu  
usermod -l zhaoliu wangwu  
// 修改用户zhangsan的用户名全称为张三  
usermod -c 张三 zhangsan  
// 修改用户zhangsan在口令过期后20天就禁用该账户  
usermod -f 20 zhangsan  
// 修改用户sun所属的组群为root，该组群必须事先存在  
usermod -g root sun  
// 锁住用户zhangsan口令，使口令无效  
usermod -L zhangsan  
// 对应解锁  
usermod -U zhangsan  
// 修改用户zhangsan账户的过期日期是2012年12月12号  
usermod -e 12/12/2012 zhangsan  
// 修改用户zhangsan的Shell类型为/bin/ksh  
usermod -s /bin/ksh zhangsan  


#### 删除用户账户

使用userdel命令可以在Linux系统下删除用户账户。  
命令语法：  
userdel [ -r ] [ 用户名 ]

> //删除用户lisi  
userdel lisi  
// 删除用户moon，并且在删除该用户的同时一起删除主目录  
userdel -r moon  

## 组群账户管理

### Linux组群账户配置文件

具有某种共同特征的用户集合就是用户组群，用户组群配置文件主要有/etc/group和/etc/gshadow，其中/etc/gshadow是/etc/group的加密信息文件。  

#### 1. /etc/group文件

例如：  
zhangsan:x:1000:  

/etc/group文件各字段的含义  

|字段|含义|
|---|---|
|组群名|用户组群名称，如组群名root|
|组群口令|存放加密的密码，在上面示例中我们看到的是一个x，其实口令已被映射到/etc/gshadow文件中|
|组群标识号|在系统内用一个整数标识组群GID，每个组群的GID都是唯一的，默认普通组群的GID从1000开始，如root组群GID是0|
|组群成员|属于这个组群的成员，如root组群的成员有root用户|  

组群GID和UID类似，是一个从0开始的正整数，GID为0的组群是root组群。

#### 2. /etc/gshadow文件

例如：  
beijing:$6$E/xvWMmh$rhYLQwwffEqIudVLFzMlv1::ou  

/etc/gshadow文件各字段和含义  

|字段|含义|
|---|---|
|组群名|组群的名称|
|组群口令|口令已经加密，如果有些组群在这里显示的是“!”，表示这个组群没有口令。上面示例中组群shanghai没有口令，组群beijing已设置口令|
|组群管理者|组群的管理者，有权在该组群中添加、删除用户|
|组群成员|属于该组群的用户成员列表，如有多个用户用“,”分割。上面示例中beijing组群的成员是ou|

### 字符界面下组群账户的设置

在Linux系统字符界面下创建、修改以及删除组群账户主要使用groupadd，groupmod和groupdel这三个命令，其结果与使用“用户管理者”工具一样。

#### 创建组群账户

使用groupadd命令可以在Linux系统下创建组群账户。  
命令语法：  
groupadd [ -g gid [ -o ] ] [ -f ] [ 组群名 ]

> // 创建名为china的组群  
groupadd china  
// 创建名为ou的组群，并且设置该组群GID为1800  
groupadd -g 1800 ou  
// 创建名为chinese的系统组群  
groupadd -r chinese  

#### 修改组群账户

使用groupmod命令可以在Linux系统下修改组群账户，如组群名称、GID等。  
命令语法：  
groupmod [ -g < 组群识别码 > < -o > ] [ -n < 新组群名称 > ] [ 组群名称 ]  

> // 将组群ou的GID修改为1900  
groupmod -g 1900 ou  
// 修改组群ou的新组群名称为shanghai  
groupmod -n shanghai ou  


#### 删除组群账户

使用groupdel命令可以在Linux系统下删除组群账户。  
命令语法：  
groupdel [ 组群名称 ]

> // 删除组群shanghai  
groupdel shanghai  


## 账户相关文件或目录

### /etc/skel目录

/etc/skel目录是存放用户启动文件的目录，这个目录由root用户管理，当管理员创建新用户时，这个目录下的文件会自动复制到新创建的用户的主目录下。 /etc/skel目录下的文件都是隐藏文件，也就是类似“.file”格式的，可以通过添加、修改和删除/etc/skel目录下的文件，来为用 户提供一个统一、标准和默认的用户环境。

### /etc/login.defs配置文件

/etc/login.defs文件规定了创建新用户时的一些默认设置，比如创建用户时是否需要主目录、UID和GID的范围、用户账户口令的期限等，这个文件可以通过root用户来修改。

### /etc/default/useradd文件

/etc/default/useradd文件是在使用useradd命令创建用户账户时的规则文件。  

## 用户和组群维护命令

### 账户维护命令

#### passwd命令

使用passwd命令可以设置或修改用户的口令，普通用户和超级权限用户都可以运行passwd。普通用户只能更改自己的用户口令，root用户可以设置或修改任何用户的口令。如果passwd命令后面不接任何选项或用户名，则表示修改当前用户的口令。  
命令语法：   
passwd [ 选项 ] [ 用户名 ]

> // 设置用户it的口令  
passwd it  
// 设置当前用户的口令  
passwd  
// 锁住用户it的口令  
passwd -l it  
// 解锁用户it口令  
passwd -u it  
// 删除用户it的口令  
passwd -d it  


#### gpasswd

使用gpasswd命令可以设置一个组群的组群密码，或是在组群中添加、删除用户。   
命令语法：   
gpasswd [ -r|-R ] [ 组群名 ] gpasswd [ 选项 ] [ 用户名 ] [ 组群名 ]

> // 把用户it添加到kk组群中  
gpasswd -a it kk  
// 从kk组群中删除用户it  
gpasswd -d it kk  
// 设置kk组群的口令  
gpasswd kk  
// 取消kk组群的密码  
gpasswd -r kk  

#### chfn命令

使用chfn命令可以更改用户全名、办公室地址、电话等信息。  
命令语法：   
chfn [ -f full-name ] [ -o office ] [ -p office-phone ] [ -h home-phone ] [ -u ] [ -v ] [ 用户名 ]

> // 更改用户newuser的信息  
chfn newuser  
// 设置用户it的办公地址是财务室  
chfn -o 财务室 it  
// 设置用户it的用户名全称为挨踢  
chfn -f 挨踢 it  

#### chsh命令

使用chsh命令可以更改用户账户的Shell类型。   
命令语法：   
chsh [ -s Shell类型 ] [ -l ] [ 用户名 ]

> // 列出当前系统中所有支持的Shell类型  
chsh -l  
// 更改用户wangwu所用的Shell类型为/bin/sh  
chsh -s /bin/sh wangwu  
// 更改当前用户wangwu的Shell类型为/bin/bash  
chsh wangwu  


#### su命令

使用su命令可以切换到其他用户账户进行登录。   
命令语法：   
su [ 选项 ] [ 用户 ]

> // 从用户root切换到用户it登录系统  
su -it  
// 从用户it切换到用户root登录系统  
su root  

#### pwck命令

使用pwck命令可以校验用户配置文件/etc/passwd和/etc/shadow内容是否合法和完整。   
命令语法：   
pwck

> // 检验用户配置文件/etc/passwd和/etc/shadow文件内容是否合法和完整  
rm -rf /home/it  

#### newgrp命令

使用newgrp命令可以让用户账户以另一个组群的身份进行登录。  
命令语法：  
newgrp [ 组群名 ]

> // 将用户ab以组群ou的身份登录系统  
newgrp ou  

### 账户信息显示

#### finger命令

使用finger命令可以显示用户账户的信息。   
命令语法：   
finger [ 选项 ] [ 用户名 ]

> // 显示所有用户的登录信息  
finger  
// 显示用户newuser的信息  
finger newuser  

#### groups命令

使用groups命令可以显示指定用户账户的组群成员身份。   
命令语法：   
groups [ 用户名 ]

> // 查看用户ab是属于哪些组群的成员  
groups ab  

#### id命令

使用id命令可以显示用户的ID以及该用户所属组群的GID。   
命令语法：   
id [ 选项 ] [ 用户名 ]

> // 查询用户ab的UID、GID以及归属组群的情况  
id ab  
// 显示用户ab所属主组群的GID  
id -g ab  
// 显示用户ab所属组群的GID  
id -G ab  
// 显示用户ab的UID  
id -u ab  

#### w命令

使用w命令可以详细查询已登录当前计算机的用户。   
命令语法：   
w

> // 显示已登录当前计算机的用户详细信息  
w  

#### who命令

使用who命令可以显示已登录当前计算机用户的简单信息。   
命令语法：   
who [ -Himqsw ] [ --version ] [ am i ] [ 记录文件 ]

> // 显示已登录当前计算机用户的简单信息  
who  


## 实现账户安全

在Linux系统中可以使用chage命令管理用户口令的时效，防止用户口令由于长时间使用而导致泄漏，或是被黑客破解口令而受到攻击。   
命令语法：   
chage [ 选项 ] [ 用户名 ]

chage命令选项含义  

|含义|选项|描述|
|---|---|---|
|两次改变密码之间相距的最小天数|-m days|指定用户必须改变口令所间隔的最少天数。如果值为0，口令码就不会过期|
|两次改变密码之间相距的最大天数|-M days|制定口令有效的最多天数。当该选项制定的天数加上“-d”选项指定的天数小于当前的日期，用户在使用该账户前就必须改变口令|
|最近一次密码修改时间|-d days|指定自从1970年1月1日起，口令被改变的天数|
|密码失效时间|-I days|指定口令过期后，账户被锁前不活跃的天数。如果值为0，账户在口令过期后就不会被锁|
|账户过期时间|-E date|指定账户被锁的日期，日期格式为YYYY-MM-DD。若不用日期，也可以使用自1970年1月1日后经过的天数|
|在密码过期之前警告的天数|-W days|指定口令过期前要警告用户的天数|

> // 设置用户shanghai两次改变密码之间相距的最小天数为2天  
chage -m 2 shanghai  
// 设置用户shanghai两次改变密码之间相距的最大天数为10天  
chage -M 10 shanghai  
// 设置用户shanghai在密码过期之前警告的天数为1天  
chage -W 1 shanghai  
// 设置用户shanghai密码失效时间为10天  
chage -l 10 shanghai  
// 设置用户shanghai账户过期时间为2018-10-10  
chage -E 2018-10-10 shanghai  
// 显示用户shanghai当前口令失效的信息  
chage -l shanghai  
// 用交互式的方式设置用户beijing的口令时效  
chage beijing  
