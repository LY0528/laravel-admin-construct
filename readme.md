## 本文只针对windows用户
** 重要** 在安装phpStudy的时候，安装路径不要有中文，安装的文件夹不要有空格；否则，在启动Apache或者MySQL的时候会出现失败的情况。
### 一、安装composer并配置php环境变量
1.1、 Laravel 使用 Composer 来管理代码依赖。所以，在使用 Laravel 之前，请先确认你的电脑上安装了 Composer。如果没有安装，请按照一下步骤安装。        
1.2、 下载并且运行[Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe),它将安装最新版本的 Composer。双击开始安装：
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/01.png" height="280" width="350">  
这里会自动搜索 php.exe 路径, 如果找不到，则手动添加路径。点击next直到安装结束  
1.3、 添加php.exe环境变量，[这里以win10系统为例](http://jingyan.baidu.com/article/ad310e80d2ebe31848f49e59.html)：  
1.3.1、 右键开始菜单--选择控制面板  
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/02.png" width="130" height="240">  
1.3.2、 切换到大图标模式   
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/03.png" width="480" height="250">  
1.3.3、 选择系统    
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/04.png" width="560" height="330">      
1.3.4、 选择高级系统        
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/05.png" width="550" height="160">       
1.3.5、 点击环境变量        
1.3.6、 点击系统变量下面的新建        
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/06.png" width="320" height="280">      
1.3.7、 输入环境变量信息      
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/07.png" width="420" height="100">      
1.3.8、 点击确定保存，打开cmd 输入 php -v 看是否配置正确 我这里是7.0.12       
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/08.png">
### 二、开启openssl扩展
2.1、 在PHP目录下，打开php.ini文件，去掉extension=php_openssl.dll前面的分号(;)   
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/09.png" width="500" height="150">      
2.2、 下载[composer.phar](https://getcomposer.org/composer.phar)并放到PHP目录下，在PHP目录下新建composer.cmd文件， 内容为

    @php "%~dp0composer.phar" %*

<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/10.png" width="320" height="150">      
2.3、 保存后，运行这个文件，打开cmd，输入 composer -V(是大写的哟)  查看是否成功
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/11.png">  
2.4、 国内网速有限，可以安装 Packagist 为国内镜像

    composer config -g repo.packagist composer https://packagist.phpcomposer.com
    
到这里我们的composer真正的安装完成了，下面开始安装Laravel        

###　三、安装 Laravel
3.1、 使用 Composer 下载 Laravel 安装包

    composer global require "laravel/installer"

请确定你已将 
        
    ~/.composer/vendor/bin

路径加到 PATH，只有这样系统才能找到 laravel 的执行文件                    
3.2、 如何添加到PATH？   
3.2.1、 首先让我们回到1.2.5步骤，然后找到PATH选项并双击    
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/12.png" width="300" height="300">      
3.2.2、 点击右边的新建，把你的~/.composer/vendor/bin路径复制粘贴进去，点击确定保存就可以了           
<img src="https://github.com/LY0528/laravel-admin-construct/raw/master/images/13.png" width="300" height="300">   

### 四、安装laravel-admin并配置
1、新建一个项目
    
    laravel new blog //blog是项目名
2、切换到项目目录，在项目目录的命令行中输入

    composer require encore/laravel-admin "1.4.*"

3、在项目目录的.env文件中进行配置

    DB_DATABASE=blog //项目名称
    DB_USERNAME=root //数据库用户名
    DB_PASSWORD=     //数据库密码
4、然后在config/app.php加入ServiceProvider:    

    Encore\Admin\Providers\AdminServiceProvider::class
5、然后运行下面的命令来发布资源：

    php artisan vendor:publish --tag=laravel-admin

6、在文件config/admin.php中，可以在里面修改安装的地址、数据库连接、以及表名。 
然后运行下面的命令完成安装：

    php artisan admin:install
7、接下来还需要配置一些文件：         
7.1、在hosts文件中最后一行添加：

    127.0.0.1  blog.com //我的项目文件夹名称是blog

hosts文件的路径:C:\Windows\System32\drivers\etc      
7.2、打开Apache中的vhosts-conf配置文件（我用的是phpStudy），添加：

    <VirtualHost blog.com:80> //记得换成你的文件名称和端口号
        DocumentRoot "F:\phpStudy\WWW\blog\public" //记得换成你的文件路径
        ServerName blog.com
    </VirtualHost>

8、启动你的laravel-admin后台管理系统      
8.1、第一种方法：      
    打开你的phpStudy，启动Apache和MySQL之后，打开浏览器，在浏览器中输入

    blog.com/admin
8.2、第二种方法：      

    使用php artisan  serve开启服务，启动服务后，会在命令行中生成一个端口号，在浏览器打开 http://  localhost:生成的端口号/admin
     ,使用用户名 admin 和密码 admin登陆.

### 五、如何修改laravel-admin默认样式
#### 一、修改laravel-admin模板字体图标         

    由于字体图标都有一个为fa的class类名。我们只需要在public/packages/admin/  font-awesome/css和vendor/encore/laravel-admin
    /assets/font-awesome/css文件夹下的css文件中给fa类名下增加一个color属性即可修改所有 字体图标的颜色。若是修改某一个字体图标的
    颜色，我们只需要找到这个字体图标对应的class类名，以同样的方法在其class类名下增加color属性即可。这种方法改动较大，不推荐这种
    做法。        
#### 二、在laravel-admin模板中自定义皮肤        
2.1、首先在public/admin/AdminLTE/dist/css/skins文件夹下新建自己的皮肤文件名，后缀css，把其其中一个皮肤所有的样式复制一份粘贴过来。        
2.2、把复制来的css文件中所有的皮肤类名全局替换成自定义的皮肤类名，如：

    复制的是skin-blue.min.css文件，这个文件的皮肤类名就是skin-blue，然后使用全局替换的方法把skin-blue类名替换成skin-myself

2.3、接下来定义属于自己的皮肤，如：

    想要修改顶部navbar的背景颜色，只需要找到定义此样式的类名，背景样式是由.skin-myself .main-header .navbar控制；所以只需在其
    类名下修改属性值即可：
    .skin-myself .main-header .navbar {
        background-color: #1f262e;
    }
    其他样式类名如：logo、main-sidebar、sidebar-menu、active、treew-menu，修改方法如法炮制。 

2.4、如何使你的皮肤生效

    完成上述步骤之后，还需要在config/admin.php文件skin选项中把皮肤名替换成自定义皮肤名。然后刷新，就会发现自定义的皮肤已经生效。 
#### 三、在laravel-admin中增加自己的样式        
3.1、给登录页面增加背景图片及修改其他样式

    在public/admin/AdminLTE/dist/css文件夹下建立一个新的css文件，（这里以myCss.css为例，改动的css样式都会放在这里）。打开调试
    工具，可以发现定义登录页面背景图片的类名是login-page和register-page，于是我们可以这样定：
    .login-page, .register-page {
        background: url(../img/beij.jpg);
        background-size: cover;
    }
    这里自定义背景图片放在了public/admin/AdminLTE/dist/img文件夹下。 
    然后，要把myCss.css文件引入到index.php和login.php文件中
    刷新页面就会发现自定义的背景图片已经生效。
    其他样式也是以同样的方法定义，若是定义属性不起作用，检查是否由于权重引起的。若是，就在属性值后面加上!important；若不是，可能
    其样式不是当前的类名控制。具体情况要具体分析。

3.2、如何修改模板登录界面的标题？  

    修改config/admin.php文件中的name项即可           
3.3、如何修改模板的logo和mini-logo标题？  

    修改config/admin.php文件中logo项和mini-logo项即可     
3.4、如何修改用户名头像？     

    单击右上角头像或者用户名，选择设置选项，在里面可以修改用户名和头像以及密码
