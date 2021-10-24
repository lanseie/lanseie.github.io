## 搭建Home page

#### 1. 创建个人网页

​	使用Github.io完成个人网页创建即可

#### 2. 安装Jekyll

​	往github.io网站中填充的内容需要本地先调整好、预览才行，否则改一步、上传看效果、再改一步，这样效率太低了。安装Jekyll就是为了本地完成网页内容的调试。

​	a. 点击安装 [Ruby installer](https://rubyinstaller.org/) 

​	b. 下载 [RubyGems](https://rubygems.org/pages/download) ，下载最新包。按照说明，使用ruby安装包：`ruby setup.rb`

​	c. 安装jekyll：

​		直接执行 gem安装命令会链接到海外官网，等很久都没下完。因此需要修改下载源：

```ruby
gem sources --remove https://rubygems.org/
gem sources -a 'https://gems.ruby-china.com'
gem sources -l
```

​		执行最后一句“gem sources -l”会看到下载源修改生效。

​		执行：`gem install jekyll`  继续安装，发现系统报如下的错。

![image-20211023223645699](C:\Users\lovig\AppData\Roaming\Typora\typora-user-images\image-20211023223645699.png)

​		很多人都反馈是mysy2没安装好，重新执行rubyinstaller-devkil把mysy2安装一遍，再安装jekyll成功。

#### 3. 安装子页面

​	参考Github标准文档，在已有的lanseie.github.io工程文件夹中创建两级子目录：UseFFmpeg\UseFFmpeg_01_LinuxCompileFFmpeg。

​	然后在第一级子目录UseFFmpeg中打开cmd命令行，运行:

​	`jekyll new UseFFmpeg_01_LinuxCompileFFmpeg --skip-bundle`

​	就会在这里产生初始的页面内容。

​	再cd到刚才生成的子目录，运行`bundle install` 。但实际本地预览失败了，报错：

C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)

​	直接在子目录下增加修改_config.yml并增加主题、预览子目录下的404.html，发现主题生效了。

​	于是直接在根目录 lanseie.github.io 的README.md中添加导向子页面的链接：

​	[true subpage](https://lanseie.github.io/UseFFmpeg/UseFFmpeg_01_LinuxCompileFFmpeg/index.markdown) 

#### 参考文献

1. [《Github+Jekyll搭建个人网站详细教程》](https://www.jianshu.com/p/9f71e260925d)
2. [《欢迎- Jekyll • 简单静态博客网站生成器》](https://jekyllcn.com/docs/home/)