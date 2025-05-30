# 更改 VSCode 背景

我们可以下载 background 这个 extension.

Cmd+shift+P 打开 setting.json, 把这个配置加入进去

其中 images 填本地想放在上面的 Images.

panel 是 terminal 那边, sidebar 是侧边, editor 是代码边

```json
	"background.enabled": true,
    "background.panel": {
        "useFront": true,
        "images": ["file:///Users/fanqiulin/Documents/Photos/scene.jpeg"],
        "opacity": 0.08,
        "width": "100%",
        "height": "100%",
        "backgroundSize": "cover",
        "backgroundPosition": "center",
        "backgroundRepeat": "no-repeat"
    },
    "background.sidebar": {
        "useFront": true,
        "images": ["file:///Users/fanqiulin/Documents/Photos/samurai.jpeg"],
        "opacity": 0.08,
        "width": "100%",
        "height": "100%",
        "backgroundSize": "cover",
        "backgroundPosition": "center",
        "backgroundRepeat": "no-repeat"
    }
```





## 仅更改颜色

也是在 setting.json

```json
"workbench.colorCustomizations": {
     "editor.background": "#252424"
}
```

这个得自己调了, 或者偷别人的配色方案
