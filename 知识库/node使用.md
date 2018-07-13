## node使用

* 检查使用包是否有新的版本

```
npm outdated
#检查全局使用包npm -g outdated
```

* 升级某个包

```
npm install package-name
#yarn upgrade package-name
```

* 升级所有包（慎用）

```
#!/bin/sh
set -e
#set -x
for package in $(npm -g outdated --parseable --depth=0 | cut -d: -f2)
do
    npm -g install "$package"
done
```