https://www.npmjs.cn/getting-started/what-is-npm/  npm快速入门

.npmrc  The npm config files.

.npmrc文件，就是npm的配置文件所在位置。当然，寻找这个文件的目的，多数是为了修改.npmrc文件内容。但npm提供了方便快捷的修改方式，不知道这个文件的位置，其实也是可以修改的。

https://blog.csdn.net/cnds123/article/details/106206232?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-5-106206232.nonecase&utm_term=npmrc%E6%96%87%E4%BB%B6&spm=1000.2123.3001.4430

一.npm配置
(1)npm cli 提供了npm config 命令进行npm相关配置, 通过npm config ls -l 可查看npm的所有配置,包括默认配置.

(2)npm config set 进行配置项修改,使用命令配置后会把配置文件中的注释全部删除

(3)一些常用配置:
proxy, https-proxy :指定npm使用代理
registry 指定npm下载安装包的源 默认:https://registry.npmjs.org/
npm config set prefix $dir

二.npmrc文件
(1)除了使用cli的npm config命令来显示修改npm配置,还可以通过npmrc文件直接修改配置

(2)npmrc文件路径
npm config ls -l 命令查看配置 存在npmrc文件就会打印出文件路径,没有的话就使用命令配置registry,npmrc文件就会出现

###### npm-access

Set access level on published packages

###### npm-adduser

Add a registry user account

###### npm-audit

Run a security audit

###### npm-bin

显示 npm 的 bin 文件夹的路径

###### npm-bugs

Bugs for a package in a web browser maybe

###### npm-build

构建一个包

###### npm-cache

Manipulates packages cache

###### npm-ci

Install a project with a clean slate

###### npm-completion

Tab Completion for npm

###### npm-config

Manage the npm configuration files

###### npm-dedupe

Reduce duplication

###### npm-deprecate

作废指定包的指定版本

###### npm-dist-tag

Modify package distribution tags

###### npm-docs

Docs for a package in a web browser maybe

###### npm-doctor

Check your environments

###### npm-edit

Edit an installed package

###### npm-explore

Browse an installed package

###### npm-help

Get help on npm

###### npm-help-search

Search npm help documentation

###### npm-hook

Manage registry hooks

###### npm-init

create a package.json file

###### npm-install

Install a package

###### npm [install-ci-test](https://www.npmjs.cn/cli/install-ci-test) 

Install a project with a clean slate and run tests

###### npm [install-test](https://www.npmjs.cn/cli/install-test) 

安装依赖包并运行测试

###### npm-link

Symlink a package folder

###### npm-logout

Log out of the registry

###### npm-ls

List installed packages

###### npm

javascript package manager

###### npm-org

Manage orgs

###### npm-outdated

Check for outdated packages

###### npm-owner

Manage package owners

###### npm-pack

Create a tarball from a package

###### npm-ping

Ping npm 注册表

###### npm-prefix

显示（目录）前缀

###### npm-profile

Change settings on your registry profile

###### npm-prune

移除无关的包

###### npm-publish

Publish a package

###### npm-rebuild

重新构建包

###### npm-repo

在浏览器中打开指定包的源码仓库页面

###### npm-restart

Restart a package

###### npm-root

###### 显示 npm 根目录

###### npm-run-script

Run arbitrary package scripts

###### npm-search

Search for packages

###### npm-shrinkwrap

锁定依赖包的版本

###### npm-star

标记你所喜欢的包

###### npm-stars

查看 star 过的包

###### npm-start

start 脚本

###### npm-stop

stop 脚本

###### npm-team

Manage organization teams and team memberships

###### npm-test

test 脚本

###### npm-token

Manage your authentication tokens

###### npm-uninstall

Remove a package

###### npm-unpublish

Remove a package from the registry

###### npm-update

Update a package

###### npm-version

Bump a package version

###### npm-view

View registry info

###### npm-whoami

显示 npm 用户名
