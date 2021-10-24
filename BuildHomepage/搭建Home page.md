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

#### 参考文献

1. [《Github+Jekyll搭建个人网站详细教程》](https://www.jianshu.com/p/9f71e260925d)
2. [《欢迎- Jekyll • 简单静态博客网站生成器》](https://jekyllcn.com/docs/home/)