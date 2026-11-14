# zyh_blog · 文件夹即节点的树形博客

纯 **GitHub Pages + Jekyll**，树形导航零插件，无需 Actions、无需本地环境。
**每个文件夹就是一个节点**，在文件夹里写文章 = 在该节点下新增文章。

```
https://<你的用户名>.github.io/zyh_blog/
└── /blog/    ← 文章树
```

> **当前状态**：仓库已推送，`_posts/` 目前为空——5 篇示例文章已归档到 [附六](#附六样例文章存档)。
> 想直接开始写作，跳到 [二、写文章](#二写文章)。

## 目录

| 你要找的 | 看这里 |
|---|---|
| 这站有哪些能力 | [一、功能介绍](#一功能介绍) |
| **怎么写一篇文章** | [二、写文章](#二写文章) |
| **怎么加 / 改 / 删节点** | [三、改节点](#三改节点) |
| 明暗主题怎么用 | [四、明暗主题](#四明暗主题) |
| 文件都在哪 | [附一：目录结构](#附一目录结构) |
| 怎么发布、日常 git | [附二：上线与日常 Git](#附二上线与日常-git) |
| 页面空白、构建失败 | [附三：排错](#附三排错) |
| 改模板前必看 | [附四：改模板前必读](#附四改模板前必读liquid-两个坑) |
| 折叠 / 排序 / 配色等开关 | [附五：可调项](#附五可调项) |
| 已清空的示例文章 | [附六：样例文章存档](#附六样例文章存档) |

---

## 一、功能介绍

| 功能 | 说明 |
|---|---|
| **文件夹即节点** | 目录结构就是站点结构。新增节点只靠建文件夹，不用注册分类、不用维护索引页、不用改配置 |
| **树形折叠导航** | `/blog/` 是一棵可折叠的文件树：同层「先子节点、后文章」，节点带篇数徽章，顶部有「全部展开 / 全部折叠」 |
| **三态明暗主题** | 跟随系统 / 浅色 / 深色，开关在**主页**，选择记在浏览器本地，刷新不闪白 |
| **纯 Markdown 写作** | front matter 只要 `title` + `date` 两行，`layout` 已自动配好 |
| **干净的文章 URL** | `/zyh_blog/2025/01/02/hello-world/`，中文目录名不会进 URL |
| **推送即发布** | `git push` 后约 1 分钟自动重建，不写 Actions、不装 Ruby |
| **自带 RSS 与 sitemap** | `/feed.xml`、`/sitemap.xml`（`jekyll-feed` + `jekyll-sitemap`，均在 GitHub Pages 白名单内） |
| **零依赖样式** | 单文件 `assets/css/style.css`，无主题 gem、无 JS 框架、无网络字体 |
| **附赠** | 文章页面包屑、404 页面、手机适配、明暗两套配色、SEO 描述与 canonical |

---

## 二、写文章

### 1. 决定放哪个节点

> **文章放在哪个文件夹里，就挂在哪个节点下。**

| 文件位置 | 树上的位置 |
|---|---|
| `_posts/2026-03-01-x.md` | 树的**根层级** |
| `随笔/_posts/2026-03-01-x.md` | `随笔` 下 |
| `技术/前端/_posts/2026-03-01-x.md` | `技术 → 前端` 下 |
| `技术/前端/工程化/_posts/2026-03-01-x.md` | `技术 → 前端 → 工程化` 下 |

层级来自「`_posts` 上层的目录名」，Jekyll 会把这些目录名自动登记为文章的 `categories`，
`_includes/tree.html` 再把它们拼成节点路径。

### 2. 文件名必须是 `YYYY-MM-DD-英文slug.md`

```text
技术/前端/_posts/2026-03-01-css-grid.md
                └────┬────┘ └──┬───┘
                  日期      英文 slug（URL 用它）
```

- 日期前缀缺了 Jekyll 不认这个文件；
- slug 用英文小写连字符，中文只写在 `title:` 里，否则 URL 会是一串 `%E4%BD%A0...`。

### 3. 正文最小写法

```markdown
---
title: CSS Grid 入门
date: 2026-03-01 10:00:00 +0800
---

正文用 Markdown 写，标题、列表、表格、代码块、图片都能用。
```

`layout: post` 不用写，`_config.yml` 里已给所有文章默认值。

### 4. 三种写作方式

- **GitHub 网页**（不用装任何东西）
  `Add file → Create new file`，文件名框里直接输入完整路径
  `技术/前端/_posts/2026-03-01-css-grid.md`（斜杠会自动建目录），
  写完 `Commit changes` 即发布。
- **本地 + git**
  ```bash
  mkdir -p 技术/前端/_posts
  # 新建 技术/前端/_posts/2026-03-01-css-grid.md 并写好内容
  git add . && git commit -m "post: CSS Grid 入门" && git push
  ```
- **先攒草稿**
  front matter 里加 `published: false`，首页和树上都不出现，想发时删掉这行。

### 5. 插入图片（可以）

**可以插入图片。** 图片放 `assets/images/`（建议按年份分子目录、文件名用英文小写），
然后在文章里这样引用：

```markdown
![树形导航截图]({{ site.baseurl }}/assets/images/2026/tree.png)
```

三个必须注意的点：

1. **不要写成 `/assets/images/…`**。本站部署在子路径
   `https://<用户名>.github.io/zyh_blog/`，以 `/` 开头会指到域名根目录 → 图片 404。
   `{{ site.baseurl }}` 会展开成 `/zyh_blog`，路径才对。
2. 等价写法（效果一样，二者选一）：
   ```markdown
   ![树形导航截图]({{ '/assets/images/2026/tree.png' | relative_url }})
   ```
3. **文件名区分大小写**（GitHub Pages 跑在 Linux 上）：`Tree.PNG` 与 `tree.png` 是两个文件，
   写错就是 404；也不要用中文文件名，会被百分号编码。

其他常用写法：

```markdown
<!-- 外链 / 图床 -->
![来自图床](https://example.com/pic.jpg)

<!-- 指定宽度（Markdown 语法不支持，用 HTML 标签） -->
<img src="{{ site.baseurl }}/assets/images/2026/tree.png" width="480" alt="树形导航">
```

- 样式里已设 `.post img { max-width: 100% }`，大图不会撑破手机屏幕；
- **上传图片**：网页端进到 `assets/images/2026/` 目录 → `Add file → Upload files` 拖进去；
  本地直接 `git add`；
- 别把图片放进 `_posts/` 里；
- 单张建议压到 300KB 以内（PNG/WebP），仓库体积和首屏速度都受影响。

### 6. 发布

```bash
git add . && git commit -m "post: 标题" && git push
```

约 1 分钟后自动生效，不需要任何构建命令。

---

## 三、改节点

节点 = 文件夹。改节点就是改文件夹结构，**全程不需要碰 `_config.yml`**。

| 想做的事 | 怎么做 |
|---|---|
| **加一个节点** | 新建文件夹 + 里面建 `_posts/` + 放文章。网页端在文件名框输入 `读书/技术书/_posts/2026-03-01-x.md` 即可 |
| **给节点加子节点** | 继续往里套：`读书/技术书/《重构》/_posts/2026-03-01-x.md` |
| **在节点下直接写文章** | 把 `.md` 放该文件夹的 `_posts/`，不用建子文件夹 |
| **节点改名** | 网页端逐个编辑文件，把文件名框里的路径前缀改成新目录名后提交 |
| **文章换节点** | 同理，只改路径前缀；**文章 URL 不变**（URL 由日期 + slug 决定） |
| **合并两个节点** | 把文章都挪进同一个文件夹 |
| **删除节点** | 删光该文件夹里的文章，节点自动从树上消失（Git 本来也不跟踪空目录） |
| **放在根层级** | 放进仓库根的 `_posts/` |

几个细节：

- **节点后面的数字**只统计直接放在该节点 `_posts/` 里的文章，不含子节点里的；
  只有子节点、自己还没有文章的目录不显示数字（例如 `技术`）。
- **同层排序**：先子节点、后文章；子节点按名称的字符编码排序（中文不是拼音序），
  文章按日期倒序。
- **层数上限 128**：`_includes/tree-node.html` 里 `depth < 128` 控制；128 实际等于不设限，想收紧就把它改小。
- **别在 front matter 里写 `categories:`**，层级只由文件夹决定；写了会和路径拼接，可能多出意外节点。

---

## 四、明暗主题

首页标题右侧三个按钮：**跟随系统 / 浅色 / 深色**。

- 点一下立即生效，选择记在浏览器 `localStorage` 的 `zyh-theme` 键里；
- **同一浏览器全站生效**，刷新不会闪白（`data-theme` 在样式表加载前就写好了）；
- 只跟浏览器走、不跟 GitHub 账号走，换设备要重新选；
- 没选过时默认「跟随系统」，由操作系统决定明暗。

想改默认值或改配色 → [附五：可调项](#附五可调项) 里的「主题默认值」「主题颜色」两行。

---

## 附一：目录结构

```
zyh_blog/
├── _config.yml                  # 站点配置（baseurl / url 在这里）
├── _layouts/
│   ├── default.html             # 骨架 + 主题初始化 + SEO
│   └── post.html                # 文章页（面包屑来自 categories）
├── _includes/
│   ├── tree.html                # 树入口：把 categories 展开成节点集合
│   └── tree-node.html           # 树递归：逐层渲染节点与文章
├── assets/
│   ├── css/style.css            # 全部样式 + 三态主题变量
│   └── images/                  # 文章图片放这里
├── index.md                     # 首页（含明暗主题开关）
├── blog.html                    # 文章树页面
├── 404.html
├── README.md
├── .gitignore
│
├── _posts/                      # ← 根层级文章（当前为空）
├── 技术/                        # ← 节点：技术
│   ├── 前端/
│   │   ├── _posts/              #   放 .md 进这里 = 「技术 → 前端」的文章
│   │   └── 工程化/
│   │       └── _posts/          #   三层节点
│   └── 后端/
│       └── _posts/
└── 随笔/
    ├── _posts/                  #   节点「随笔」自己的文章
    └── 建站/
        └── _posts/
```

> 各 `_posts/` 目录目前都是空的（示例文章见附六）。**空目录不会被 git 跟踪**，
> 所以 push 后在 GitHub 上看不到 `技术/`、`随笔/` 这些文件夹；
> 本地留着当骨架，往里放第一篇文章时它们才会出现在仓库里。

---

## 附二：上线与日常 Git

### 首次上线

1. GitHub 新建仓库 **`zyh_blog`**，可见性 **Public**。
2. 推送：
   ```bash
   git init && git add . && git commit -m "init: tree blog"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/zyh_blog.git
   git push -u origin main
   ```
3. **Settings → Pages** → Source `Deploy from a branch` → `main` / `/ (root)` → Save。

> ⚠️ `_config.yml` 的 `url` 要改成自己的用户名（只影响 RSS / sitemap）；
> `baseurl: "/zyh_blog"` **不要动**，改了整站样式会 404。

### 日常提交（推完约 1 分钟自动发布）

```bash
git status -sb                # 看改了什么
git add -A                    # 暂存（连删除一起）
git commit -m "post: 标题"     # 提交
git push                      # 推送，触发 GitHub Pages 重建
```

提交信息建议：`post:` 文章、`config:` 配置、`style:` 样式、`feat:` 功能、`fix:` 修 bug。

### 改错了怎么撤

| 阶段 | 命令 |
|---|---|
| 改了还没 add | `git restore <文件>` |
| 已 add 没 commit | `git restore --staged <文件>` |
| 已 commit 没 push | `git reset --soft HEAD~1` |
| 已经 push 了 | `git revert HEAD && git push` |

### Windows 小设置（建议做一次）

中文目录在 git 里会显示成 `"\346\212\200\346\234\257/"` 这种转义，执行一次就正常：

```bash
git config core.quotepath false
```

---

## 附三：排错

**文章树是空的**
看 `/blog/` 上方「共 N 篇」。N=0 说明文章没被识别：文件名缺 `YYYY-MM-DD-` 前缀、
front matter 没用三根 `---` 包住、或文件不在 `_posts` 目录里。

**文章都堆在根层级、没有文件夹**
`categories` 没拿到目录名。补救：把文章移回根 `_posts/`，在 front matter 加
`categories: [技术, 前端]`（顺序与目录一致），树不用改，效果一样。

**图片显示不出来**
看是不是漏了 `{{ site.baseurl }}`（写成了 `/assets/...`），或者文件名大小写不一致。

**想在文章里原样显示 `{{ }}`**
用 `{% raw %}{{ 原样 }}{% endraw %}` 包起来，否则会被 Liquid 先执行。

**整站没样式 / 链接 404**
`baseurl` 和仓库名不一致。

**构建失败**
仓库 **Actions → `pages build and deployment`** 看日志，会指出文件与行号。

---

## 附四：改模板前必读（Liquid 两个坑）

`_includes/tree-node.html` 是**递归模板**：

1. Liquid 的 `assign` 写入的是**最外层作用域**，递归子调用会覆盖父级同名变量。
   所以模板里每个变量都在「紧挨着使用前」重新赋值，绝不跨递归复用。
   破坏这个约束会出现「深层节点渲染到错误位置」。
2. **不能给 `for` 循环变量重新赋值**（`{% assign cat = cat | strip %}` 无效），
   必须换个新名字（模板里的 `s`）。

同理，`{% if %}` 里不能写过滤器，必须先 `assign` 到变量再比较。

---

## 附五：可调项

| 需求 | 位置 |
|---|---|
| 节点默认折叠 | `_includes/tree-node.html`：`<details open>` → `<details>` |
| 调整层数上限 | 同文件：`depth < 128`（现为 128，实际等于不设限） |
| 同层文章手动排序 | 文章加 `order: 1`，`tree-node.html` 两处 `site.posts` → `site.posts \| sort: "order"` |
| 主题默认值 | `_layouts/default.html`：`localStorage.getItem('zyh-theme') \|\| 'auto'` 改成 `'light'` / `'dark'` |
| 主题颜色 | `assets/css/style.css` 顶部**三处**变量块（`:root`、`html[data-theme="dark"]`、`@media` 里的 auto），三处必须同步改 |
| 目录也进 URL | `_config.yml` 的 `permalink` 加 `:categories`（中文会被百分号编码，不建议） |
| 首页也显示整棵树 | 把 `blog.html` 的 `{% include tree.html %}` 抄进 `index.md` |
| 加评论 | [giscus](https://giscus.app)（GitHub Discussions，贴段 script 即可） |

---

## 附六：样例文章存档

> `_posts/` 已清空，原先 5 篇示例文章的内容完整保存在这里。
> 恢复某篇时，把对应代码块里的内容原样存成 `.md` 文件放回原路径即可
> （文件名必须是 `YYYY-MM-DD-英文slug.md`）。
>
> 不需要这份存档时，把「附六」整段删掉即可——README 不参与构建，删了不影响站点。

### 1. 你好，世界

- 原路径：`随笔/建站/_posts/2025-01-02-hello-world.md`
- 节点：`随笔 → 建站`
- 日期：2025-01-02

````markdown
---
title: 你好，世界
date: 2025-01-02 09:00:00 +0800
---

这个文件放在 `随笔/建站/_posts/` 里，于是它在树上就长这样：

```
随笔
└── 建站
    └── 📄 你好，世界
```

## 规则只有一条：文件夹 = 节点

Jekyll 会把 `_posts` **上层**的每一级目录名登记为这篇文章的 `categories`，
本站的树就是拿 `categories` 逐级拼出来的。

| 文件放这里 | 树上的位置 |
|---|---|
| `随笔/_posts/x.md` | `随笔` 下 |
| `随笔/建站/_posts/x.md` | `随笔 → 建站` 下 |
| `技术/前端/工程化/_posts/x.md` | `技术 → 前端 → 工程化` 下 |
| `_posts/x.md`（根目录） | 树的根层级 |

所以「在文件夹下写文章」真的就是「在节点下新建文章」——
**不需要改任何配置，也不需要维护目录索引**。
某个文件夹里的文章被删光，那个节点会自动从树上消失。
````

### 2. CSS 变量与主题切换

- 原路径：`技术/前端/_posts/2025-01-03-css-vars.md`
- 节点：`技术 → 前端`
- 日期：2025-01-03

````markdown
---
title: CSS 变量与主题切换
date: 2025-01-03 10:00:00 +0800
---

本文挂在 `技术 → 前端` 节点下。

## 三态主题：跟随系统 / 浅色 / 深色

`assets/css/style.css` 里把颜色全部收进 CSS 变量，然后用两个选择器切换：

```css
/* 浅色（默认） */
:root { --bg: #ffffff; --fg: #1f2328; }

/* 手动选了深色 */
html[data-theme="dark"] { --bg: #0d1117; --fg: #e6edf3; }

/* 选了「跟随系统」时，看系统偏好 */
@media (prefers-color-scheme: dark) {
  html[data-theme="auto"],
  html:not([data-theme]) { --bg: #0d1117; --fg: #e6edf3; }
}
```

## 为什么不会闪一下

`_layouts/default.html` 的 `<head>` 里有一小段内联脚本，
在样式表加载**之前**就把 `data-theme` 写到 `<html>` 上，
所以刷新页面不会先亮后暗。

选择结果存在 `localStorage` 的 `zyh-theme` 键里。
想换键名，两处一起改：`default.html` 的 head 脚本 + `index.md` 里的切换脚本。

切换面板按你的要求放在**主页**（首页顶部右侧）。
````

### 3. 用 Git 管好你的分支

- 原路径：`技术/后端/_posts/2025-01-05-git-branch.md`
- 节点：`技术 → 后端`
- 日期：2025-01-05

````markdown
---
title: 用 Git 管好你的分支
date: 2025-01-05 10:00:00 +0800
---

本文挂在 `技术 → 后端` 节点下，和 `技术 → 前端` 是兄弟节点。

| 分支 | 用途 | 谁能推送 |
|---|---|---|
| `main` | 随时可发布 | 仅合并 |
| `feat/*` | 新功能 | 开发者自己 |
| `fix/*` | 修 bug | 开发者自己 |

```bash
git switch -c feat/tree-view
git commit -am "feat: 树形导航"
git push -u origin feat/tree-view
```

对博客来说更简单的约定：**文章改动直接推 `main`**，
大改模板（layout / include / css）才开分支，确认没问题再合并。
````

### 4. 三层节点是怎样展开的

- 原路径：`技术/前端/工程化/_posts/2025-01-08-jekyll-tree.md`
- 节点：`技术 → 前端 → 工程化`
- 日期：2025-01-08

````markdown
---
title: 三层节点是怎样展开的
date: 2025-01-08 10:00:00 +0800
---

这个文件在 `技术/前端/工程化/_posts/` 里，用来验证多层节点：

```
技术
└── 前端
    ├── 📄 CSS 变量与主题切换
    └── 工程化
        └── 📄 三层节点是怎样展开的
```

`_includes/tree-node.html` 是递归模板，`depth < 128` 是安全阀；
128 实际等于不设限，想收紧就把这个数字改小。

## 加一层要做什么

只是新建一个目录：

```bash
mkdir -p 读书/技术书/_posts
# 然后在 读书/技术书/_posts/ 里写 2025-02-01-xxx.md
```

不需要动 `_config.yml`，不需要注册分类，也不需要建索引页。
````

### 5. 关于我

- 原路径：`随笔/_posts/2025-01-10-about.md`
- 节点：`随笔`
- 日期：2025-01-10

````markdown
---
title: 关于我
date: 2025-01-10 08:00:00 +0800
---

这个文件直接在 `随笔/_posts/` 下，没有更深一层，
所以它在树上就是 `随笔` 节点里的一篇文章，和子节点 `建站` 并列：

```
随笔
├── 📄 关于我
└── 建站
    └── 📄 你好，世界
```

同层永远是「先子文件夹、后文章」，各自按名称/日期排序——
这就是它看起来像文件管理器的原因。
````
