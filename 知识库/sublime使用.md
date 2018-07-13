### Sublime Text 3 使用

1) 安装Package Control插件

Package Control插件主要用来管理Sublime的插件，可以进行在线安装、卸载、启用、禁用等。

> 打开console命令行：Ctrl+`

> 输入以下命令：

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

2) 设置sidebar字体大小行间距

sidebar默认字体大小及行间距比较小，默认的配置文件中不能直接修改，所以这里借助插件实现。

依赖插件：PackageResourceViewer

> 打开 Command Palette: Ctrl+Shift+p
> 输入 Package Control: Install Package 回车，这里需要等待加载package列表
> 输入 PackageResourceViewer 回车，Package Control将会自动安装PackageResourceViewer插件
> 打开 Command Palette: Ctrl+Shift+p
> 输入 PackageResourceViewer: Open Resource 回车
> 输入 Theme - Default 回车
> 选择 Default.sublime - default,即可打开相关配置文件
> 搜索 sidebar_label ，在对应配置块下添加font.size属性,修改完成后保存即可实时应用

```
{
 "class": "sidebar_label",
 "color": [0, 0, 0],
 "font.bold": false,
 "font.italic": false, // <-- add comma
 "font.size": 16 // <-- new line
 // , "shadow_color": [250, 250, 250], "shadow_offset": [0, 0]
}
```

> 搜索 sidebar_tree,在对应配置块下添加row_padding属性，可以调整行间距

```
    {
        "class": "sidebar_tree",
        "row_padding": [5, 7],// <-- 行间距
        "indent": 12,
        "indent_offset": 17,
        "indent_top_level": false,
        "layer0.tint": [230, 230, 230],
        "layer0.opacity": 1.0,
        "dark_content": false
    },
```