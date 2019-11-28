# git-commit-helper

自动校验git提交日志的格式规范，拒绝随意提交日志，支持自动输出 `changelog` 规范。
同时支持与禅道关联，自动记录禅道日志或者完成禅道任务、BUG任务等。

__重要__

* 如果本地git低于 `2.15` 的话需要更新到最新的git。可以从 https://git-scm.com/downloads 下载。目前已知 `1.95` 版本不支持此脚本！
* 针对 [禅道专业版 7.5.1](https://www.zentao.net/) 

## 安装

#### 项目安装

将 `commit-msg` 放置到项目目录下的 `.git/hooks` 目录内

#### 全局安装

__注意：对新建项目有效，对原有项目无效！__

将 `commit-msg` 放置到 `~/.git-template/hooks` 目录内

```bash
mkdir -p ~/.git-template/hooks
git config --global init.templatedir '~/.git-template'
```

#### 权限

需要执行权限

```bash
chmod +x commit-msg
```



## 配置

修改脚本内的三个变量为实际的值

```bash
ZT_HOST="实际禅道访问地址"
ZT_USER="禅道帐号"
ZT_PASS="禅道密码"
PROJECT_XCODEPROJ_FILE="xxxx.xcodeproj/project.pbxproj" # XCode 项目文件, 用来查询版本号
VERSION_NAME="iOS_MARKETING_VERSION_BRANCH_NAME_build_XXXX" # 执行时，其中的 MARKETING_VERSION 会换成当前的版本号；BRANCH_NAME 会换成当前的分支名 (分支 feature/A/1 的名称是 1）
```

## 功能

对提交日志按如下格式进行规范校验，拒绝不符合规范的提交

```bash
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<Code Reviewer: xxxx>
<BLANK LINE>
<footer>
```

`Merge` 和 `Revert ` 的提交不进行规范校验

#### 日志Type说明

日志允许如下这些日志分类，方便输出 `release note` 或 `changelog`

```bash
  feat:
    新的功能特性
    a new feature
  fix:
    BUG修复
    a bug fix
  docs:
    文档更新
    Documentation only changes
  style:
    与代码逻辑无关的样式修改（空格、格式、句尾结束符等）
    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
  refactor:
    代码重构，但与修复bug或者新功能无关
    A code change that neither fixes a bug nor adds a feature
  perf:
    性能优化
    A code change that improves performance
  test:
    测试用例
    Adding missing tests
  chore:
    工程化更新（编译脚本、辅助工具等）
    Changes to the build process or auxiliary tools and libraries such as documentation generatio
```

#### 自动执行禅道事务（可选）

要让git提交可以自动执行禅道内的事务操作，如关闭bug，完成需求，记录工作日志等，这些都在 `<Scope>` 内来标记。

- `@1001` 完成一个需求任务
- `@1001-commit` 在一个需求任务内记录工作日志
- `#2001`完成一个BUG
- `#2001-commit` 在一个BUG内记录工作日志

在 `<scope>` 内可以填写多个指令，指令之间以 `|` 号分隔

#### 示例

一个最简单的提交日志：

```bash
docs: 接口文档更新
```

提交一个新功能，同时关闭9528号禅道任务和9000号禅道BUG，并在9527号禅道任务中记录工作日志，消耗1小时，剩余2小时，日志内容即提交日志内容。

```bash
feat(@9528|@9527-commit|#9000): 完成留言回复功能，并解决留言无法回复问题

可以引用回复，引用内容截取
被回复者会收到站内短信通知
```

## 问题

- no

## Thanks

- http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html
- https://github.com/tommarshall/git-good-commit
- https://github.com/Michael-F-Bryan/auto-changelog
