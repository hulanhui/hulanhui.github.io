---
title: Hexo使用NexT主题
date: 2018-09-14 16:20:22
tags: Hexo
---

### **Hexo使用NexT主题** 

#### 安装

在hexo安装目录下，右键：git base here，输入命令：

`git clone https://github.com/iissnan/hexo-theme-next themes/next `

#### 启用主体

所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 站点配置文件( _config.yml)， 找到 `theme` 字段，并将其值更改为 `next` 

`theme: next `

#### 主题设定

Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观 。

目前 NexT 支持三种 Scheme，他们是： 

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

`scheme: Pisces `

#### 设置语言

`language: zh-Hans `

| 语言         | 代码                 | 设定示例                            |
| ------------ | -------------------- | ----------------------------------- |
| English      | `en`                 | `language: en`                      |
| 简体中文     | `zh-Hans`            | `language: zh-Hans`                 |
| Français     | `fr-FR`              | `language: fr-FR`                   |
| Português    | `pt`                 | `language: pt` or `language: pt-BR` |
| 繁體中文     | `zh-hk` 或者 `zh-tw` | `language: zh-hk`                   |
| Русский язык | `ru`                 | `language: ru`                      |
| Deutsch      | `de`                 | `language: de`                      |
| 日本語       | `ja`                 | `language: ja`                      |
| Indonesian   | `id`                 | `language: id`                      |
| Korean       | `ko`                 | `language: ko`                      |

#### 设置作者昵称

编辑 站点配置文件， 设置 `author` 为你的昵称。 

#### 设置站点描述

编辑 站点配置文件， 设置 `description` 字段为你的站点描述。站点描述可以是你喜欢的一句签名 

#### 设置菜单

> 设定菜单内容，对应的字段是 `menu`。 菜单内容的设置格式是：`item name: link`。其中 `item name `是一个名称，这个名称并不直接显示在页面上，她将用于匹配图标以及翻译 

```
menu:
  home: /
  archives: /archives
  #about: /about
  #categories: /categories
  tags: /tags
  #commonweal: /404.html
```

> NexT 默认的菜单项有（加粗 的项表示需要手动创建这个页面）：

| 键值           | 设定值                        | 显示文本（简体中文） |
| -------------- | ----------------------------- | -------------------- |
| home           | `home: /`                     | 主页                 |
| archives       | `archives: /archives`         | 归档页               |
| **categories** | **`categories: /categories`** | **分类页**           |
| **tags**       | **`tags: /tags`**             | **标签页**           |
| **about**      | **`about: /about`**           | **关于页面**         |
| **commonweal** | **`commonweal: /404.html`**   | **公益 404**         |

> 设置菜单项的显示文本 

在第一步中设置的菜单的名称并不直接用于界面上的展示。Hexo 在生成的时候将使用 这个名称查找对应的语言翻译，并提取显示文本。这些翻译文本放置在 NexT 主题目录下的 `languages/{language}.yml`（`{language}` 为你所使用的语言）。

以简体中文为例，若你需要添加一个菜单项，比如 `something`。那么就需要修改简体中文对应的翻译文件`languages/zh-Hans.yml`，在 `menu` 字段下添加一项：

```
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  commonweal: 公益404
  something: 有料
```

> 设定菜单项的图标 

设定菜单项的图标，对应的字段是 `menu_icons`。 此设定格式是 `item name: icon name`，其中 `item name` 与上一步所配置的菜单名字对应，`icon name` 是 Font Awesome 图标的 名字。而 `enable` 可用于控制是否显示图标，你可以设置成 `false` 来去掉图标。 

```
menu_icons:
  enable: true
  # Icon Mapping.
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
```

#### 设置侧栏

通过修改 主题配置文件 中的 `sidebar` 字段来控制侧栏的行为。侧栏的设置包括两个部分，其一是侧栏的位置， 其二是侧栏显示的时机。 

> 设置侧栏的位置 

```
sidebar:
  position: left
#left - 靠左放置
#right - 靠右放置
```

> 设置侧栏显示的时机 

```
sidebar:
  display: post
#post - 默认行为，在文章页面（拥有目录列表）时显示
#always - 在所有页面中都显示
#hide - 在所有页面中都隐藏（可以手动展开）
#remove - 完全移除
```

#### 设置头像

编辑 主题配置文件， 修改字段 `avatar`， 值设置成头像的链接地址 

| 地址             | 值                                                           |
| ---------------- | ------------------------------------------------------------ |
| 完整的互联网 URI | `http://example.com/avatar.png`                              |
| 站点内的地址     | 将头像放置主题目录下的 `source/uploads/` （新建 uploads 目录若不存在）  配置为：`avatar: /uploads/avatar.png`或者 放置在 `source/images/` 目录下  配置为：`avatar: /images/avatar.png` |



更多配置 参考文章：

[NexT官方配置文档](http://theme-next.iissnan.com/getting-started.html)

[使用Hexo+Github一步步搭建属于自己的博客](https://www.cnblogs.com/fengxiongZz/p/7707568.html)