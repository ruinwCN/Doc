# Angular提交信息规范



你还在为筛选git长长的提交记录和信息烦恼么？最近发现有些仓库的提交信息杂乱无章，不容易找到自己想要的，果然看到了面向大部分开发者适用的angular git 提交规范。之后的提交就使用这个规范，摆脱需要找信息的痛苦。

## 1. 对应格式

```
<type>[optional scope]: <description>
# 空行
[optional body]
# 空行
[optional footer]
```

### 1.1  type

- feat：新功能（feature）
- fix：修补bug
- docs：文档
- style： 主要是代码风格相关的提交，比如格式化等。
- refactor：重构代码，对已有功能的重构，但是区别于bugfix。
- test：增加测试
- chore：构建过程或辅助工具的变动

每次提交声明提交的`type`是必须的，它让本次提交的作用一目了然。

#### scope（可选）

用来表明本次提交影响的范围，方便快速定位。你可以写明影响的是哪个模块（通常是模块名称）或者是哪个层（数据层、服务层、还是视图层）。

#### subject

修改版权信息

#### body（可选）

就是比较详细描述本次提交涉及的条目，罗列代码功能，这里胖哥习惯用**markdown**的列表语法，也就是用中划线换行隔开条目。当然`body`不是必选的，如果`subject`能够描述清楚的话。

#### foot（可选）

描述与本次提交相关联的**break change**或**issue** 。

#### break change

指明本次提交是否产生了破坏性修改，类似版本升级、接口参数减少、接口删除、迁移等。如果产生了上述的影响强烈建议在提交信息中写明**break change**，有利于出问题时快速定位，回滚，复盘。

#### issue

如果发现项目有bug、或者有优化的建议、甚至新增一个任务，就可以利用**issue**给项目提交一个任务。

### 1.2 Body

Body 部分是对本次 commit 的详细描述，可以分成多行。

### 1.3 Footer

Footer 部分只用于两种情况。

**不兼容变动**

**关闭 Issue**

### 1.4 Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以`revert:`开头，后面跟着被撤销 Commit 的 Header。



## 2.0 CHANGELOG

我们在 changeling 里使用这三个标志: new features, bug fixes, breaking changes.

在做一个发行版的时候，这个列表可以通过脚本来生成，通过关联这些相关的提交。

自然，你可以在实际发行之前编辑这些变更日志，但是这样子可以很容易的生成骨架。

列出自最近一次发行以来所有的主体（提交信息第一行）

```
>> git log <last tag> HEAD —pretty=format:%s
```

在本次发行内的新功能

```
>> git log <last release> HEAD —grep feature
```





参考：

> https://zhuanlan.zhihu.com/p/346127344
>
> http://www.makeiteasy.site/2020/02/19/git-commit-message-rule.html