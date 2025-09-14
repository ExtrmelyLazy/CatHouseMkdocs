# MkDocs使用指南
> 参考资料<br>
[MkDocs官方文档](https://www.mkdocs.org)

MkDocs是一款十分强大的开发工具，但部署稍有难度，根据个人经验，总结如下

## 获取

### 前提
首先，确保你已安装python和git
``` bash
pkg install python git
```

### 正式获取
1. 下载MkDocs
``` bash
pip install mkdocs mkdocs-material
```
这一步下载了MkDocs与Material主题。
> 说明<br>
Material主题是MkDocs中功能最丰富的主题，使用极其广泛。

### 创建项目
创建一个文件夹<br>
1. 直接创建
在内部存储中直接新建。<br>
2. 命令行创建
``` bash
cd ~/storage/shared/Documents
# 创建一个项目
mkdocs new my-site
cd my-site
```

## 链接与推送

### 初始化

``` bash
git init
git config --global user.email "your email"
git config --global user.name "your user name"
```

### 设置安全目录
> 警告！<br>
必须设置，不然仓库不信任

``` bash
git config --global --add safe.directory /storage/emulated/0/your path
```

## 工作流
> 如果不使用工作流，那么每次都需要`git push`，特别麻烦

### 创建工作流文件
1. 路径
```
my-site/.github/workflows/ci.yml
```
ci.yml就是工作流文件<br>
2. 配置
在ci.yml中填写：<br>
``` yaml
name: ci
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
      - run: pip install mkdocs mkdocs-material
      - run: mkdocs gh-deploy --force
```

## 推送到仓库
``` bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/user name/repo name.git
git push -u origin main
```
*URL为示例，实际不存在*

## 部署
推荐使用Netlify进行部署<br>
默认你已成功新建项目<br>
打开Site Settings,找到Deploys项<br>
在Biult command中填写`pip install -r requirements.txt && python -m mkdocs build`<br>
在Publish directory中填写`site`<br>
然后回到MkDocs文件夹，创建`requirements.txt`，内容填写<br>
``` 
mkdocs==1.5.3
mkdocs-material==9.5.17
```

## 其他配置
当你使用`git`成功下载MkDocs后，你需要配置它的`mkdocs.yml`文件<br>
以下给出一个示例配置：<br>
``` yaml
site_name: 站点名称
site_url: https://www.example.com/ #这里填写你部署的网站链接，在Netlify上可以看到

theme:
  name: material
  features:
    - navigation.tabs
    - announcement.dismiss

markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences

plugins:
  - search

nav: # 这里是导航
  - 首页: index.md
  - 关于: about.md
```

## 检查与调试
1. 网站站点显示`404 Not Found`。<br>
检查上文使用Netlify部署那里，是否设置Built commant等
2. 提交修改但网站没有反应。<br>
在仓库那里看`Action`，如果`Action`里面的`Works run`显示红色的叉，请检查你的工作流配置文件`ci.yml`，如果没有`works run`，请创建检查配置文件，或提交错误的Deploys日志问我或者AI(doge)。

## 反馈
如果检查到本教程有错误，请发送电子邮件到477522066@qq.com