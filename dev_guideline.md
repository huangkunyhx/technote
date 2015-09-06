# 必读资料书籍

- 《代码整洁之道》
- 《深入理解计算机系统》
- 《TCP/IP 详解 卷1》
- [Git相关](https://github.com/inetfuture/technote/blob/master/git.md)
- [the-art-of-command-line](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)

# 必备工具

- VPN：http://gjsq.me/6292696

    使用方法：

    - Linux PPTP：https://www.igreenjsq.mobi/shiyong/67.html
    - OS X：https://www.igreenjsq.mobi/mac.html

    VPN 是全局生效的，为了可以继续访问本地局域网（比如 192.168 网段）需要修改路由表：

    - Linux：`sudo route add -net 192.168.0.0 netmask 255.255.0.0 gw ${GATEWAY}`
    - OS X：`sudo route -n add 192.168.0.0/16 ${GATEWAY}`

    另外也可以将一些国内常用网站 IP 加进去绕过 VPN 提高访问速度，你可以把这些命令保存到一个 shell 脚本里，开机的时候自动或手动执行一下。

- Sublime Text：http://www.sublimetext.com/3

    安装 Package Control：https://packagecontrol.io/installation ，必备插件：GitGutter，knockdown，SublimeLinter

    可以将整个配置目录放到 GitHub 上，方便在多台机器上同步（也可以用 Dropbox 同步，参考下面），比如 https://github.com/inetfuture/sublime-config ，配置目录的位置：

    - Linux：~/.config/sublime-text-3/Packages/User
    - OS X：~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User

- Dropbox：https://db.tt/N5tpOzTY

    全客户端支持，包括 Linux ，需要翻墙。国内类似的服务且支持 Linux 的，我试过金山快盘，可惜同步不稳定，经常出错。

    除了同步各种资料外，还可以用来同步一些配置文件，比如各种 dot files：~/.bashrc，~/.pep8，~/.gitconfig 等等，把它们放到 Dropbox 的同步目录下，然后建立软链接到本来的位置，这样公司、家里多台机器可以无缝同步，尤其重装系统或者换新机器的时候，各种用习惯了的配置可以快速重新启用。

- 印象笔记：https://www.yinxiang.com/

    不建议使用 Evernote 国际版，太卡，而且国内的版本提供的本地化服务更方便。

    - 剪藏浏览器插件：https://appcenter.yinxiang.com/app/evernote-webclipper/web-apps/
    - 关注微信公众号：“我的印象笔记”，可以快速收藏微信内容

# 做事原则

- 认真负责
    - 每一份需求都要理解之后再动手，需求不合理的提出来讨论，对产品要有责任心，不仅仅是 “按部就班” 得做事情。
    - 提交前仔细测试自己实现的功能，检查代码细节，有交付高质量代码的责任心，而不是依赖其他成员帮你 Reivew 或者 QA 帮你发现 bug 。
- 主动沟通而不是被动应付
    - 自己的代码被别人 Review 时，如果觉得某段代码可能有问题或者不是最优方案，主动提出来讨论，而不是等待审查者询问。前者可以使 Review 更高效，更利于培养同事间的相互信任，后者一是效率低，同时也是一种不负责任的表现。
    - 进度存在风险时主动知会其他人，而不是默默拖到最后一刻连累大家加班。
- 自我驱动
    - 自觉深入学习相关技术，而不是用多少学多少，不催促不学习。
    - 主动思考产品的走向，自觉进行知识储备、调研。
    - 自觉重构低质量代码，保证项目的健康发展。
- 换位思考
    - 使用邮件、微信等工具交流时一次性提供必要的上下文，避免低效率的沟通，想一下，我这样描述对方是否可以理解并直接回复。
    - 无论是做 Code Review 还是提交功能给 QA 测试，尽自己最大努力保证质量。

# Code Review

## 流程

1. 提交者发起功能分支到 develop 分支的 Merge Request
    - 代码变动要尽量小且专注于某一个任务，不要攒的很大，或者做多个任务，要保证审查者可以较快、较容易的 Review 。
    - 发起后，要在 GitLab 上 double check 变更集。
2. 审查者 Review 代码
    - 在任何有疑问或建议的地方留 comment
    - 从中学习一些好的东西
    - 完成后，留 comment “Reviewed and waiting for fix” 或者 “Reviewed”
3. 提交者响应 comments
    - 确实有问题的，修复之
    - 不同意的，讨论
    - 完成后，留 comment “Fixed”，审查者再次检查，回到第二步。
4. 审查者确认没有问题之后，将 Merge Request 转发给 develop 分支的维护者进行合并。

## 检查清单

- 交给别人 Review 之前一定要自己先按此清单过一遍，别人只是帮你查漏补缺，对自己的代码负责，不要浪费别人的时间。
- 可读性，是否容易理解，命名是否具有足够描述性，是否有歧义，代码路径、结构是否清晰、简洁。
- Coding Style，如果有要求，应该严格遵循，例外的情况需要讨论决定。
- 一致性，包括但不限于标识符命名、错误处理、日志格式、文件组织方式、HTTP API 接口设计、UI 交互等各个方面，越是一致的系统越容易上手，越容易维护，反之则维护成本越高。
- 健壮性，是否进行必要的输入验证，边界情况是否考虑到，异常处理是否周全，会不会产生内存泄露，等等。
- 性能，数据量大或者访问频繁时是不是会有问题，对内存、数据库的使用是否高效，算法是否最优。
- 安全性，是否进行必要的权限检查，是否过度信任客户端输入。
- 重复代码，复制、粘贴的行为是要坚决禁止的，不知道如何复用代码的要主动与其他成员讨论。
- 单一职责原则，一个类、文件或者模块是否做的太多，是否干了它不该干的事。
- 开放、封闭原则，是否方便扩展，是否考虑到了以后的需求。
- 代码改动方式是否合适，是不是在一味得堆砌代码，是否需要停下来进行重构。