# 个人主页维护指南

## 目录结构

```
D:\homepage\
├── content/                 # 网站内容
│   ├── _index.md           # 首页配置
│   ├── experience.md       # 经历页面配置
│   └── publications/       # 论文目录
│       ├── _index.md
│       └── 论文文件夹/
│           └── index.md
├── data/
│   └── authors/
│       └── me.yaml         # 个人信息
├── config/_default/
│   ├── params.yaml         # 网站参数
│   └── menus.yaml          # 导航菜单
└── assets/media/           # 图片资源
```

## 1. 修改个人信息

编辑 `data/authors/me.yaml`:

```yaml
name:
  display: Kaiyuan Zhang    # 显示名称
  given: Kaiyuan
  family: Zhang

role: PhD Student           # 职位/身份
bio: |                      # 个人简介
  Your bio here...

affiliations:               # 所属机构
  - name: Harbin Institute of Technology
    url: https://www.hit.edu.cn/

links:                      # 联系方式链接
  - icon: at-symbol
    url: mailto:your@email.com
  - icon: brands/github
    url: https://github.com/username
  - icon: academicons/orcid
    url: https://orcid.org/xxxx

education:                  # 教育经历
  - area: PhD in Computer Science
    institution: 哈尔滨工业大学
    date_start: 2021-09-01
    date_end: ''
```

## 2. 添加新论文

### 步骤 1: 创建论文文件夹

在 `content/publications/` 下创建新文件夹，例如 `my-new-paper/`

### 步骤 2: 创建 index.md

在文件夹中创建 `index.md`，内容模板:

```yaml
---
title: '论文标题'

authors:
  - me                      # 自己用 me
  - Author Two              # 其他作者直接写名字
  - Author Three

date: '2025-01-01T00:00:00Z'   # 发表日期

# 论文类型:
# paper-conference (会议论文)
# article-journal (期刊论文)
# preprint (预印本)
publication_types: ['paper-conference']

publication: '*会议/期刊全称*'
publication_short: '*缩写*'

abstract: 摘要内容...

summary: 一句话总结

tags:
  - Tag1
  - Tag2

featured: true              # 是否为精选论文

hugoblox:
  ids:
    doi: 10.xxxx/xxxxx      # DOI号

links:
  - type: source
    url: https://doi.org/10.xxxx/xxxxx

projects: []
slides: ""
---
```

### 可选: 添加论文PDF

将PDF文件命名为与文件夹同名放入，例如:
- `content/publications/my-new-paper/my-new-paper.pdf`

## 3. 添加项目 (可选)

### 步骤 1: 启用项目板块

编辑 `content/_index.md`，添加项目区块:

```yaml
sections:
  # ... 其他区块 ...
  - block: collection
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - projects
    design:
      view: article-grid
```

编辑 `config/_default/menus.yaml` 添加菜单项:

```yaml
main:
  - name: Projects
    url: /#projects
    weight: 15
```

### 步骤 2: 创建项目

在 `content/projects/` 下创建文件夹，例如 `my-project/`

创建 `index.md`:

```yaml
---
title: 项目名称
summary: 项目简介
tags:
  - AI
  - Research
date: '2025-01-01'
image:
  filename: featured.jpg    # 可选封面图
---

项目详细描述...
```

## 4. 本地预览

```bash
cd D:\homepage
hugo server
```

访问 http://localhost:1313 查看效果

## 5. 部署到 GitHub Pages

### 首次设置

1. 在 GitHub 创建仓库 `username.github.io`
2. 初始化本地仓库:

```bash
cd D:\homepage
git init
git remote add origin https://github.com/username/username.github.io.git
```

3. 创建 `.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo site

on:
  push:
    branches: [main]

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

4. 在 GitHub 仓库 Settings > Pages 中设置 Source 为 `gh-pages` 分支

### 日常更新

```bash
git add .
git commit -m "描述更新内容"
git push origin main
```

GitHub Actions 会自动构建并部署

## 常用文件速查

| 修改内容 | 文件位置 |
|---------|---------|
| 个人信息 | `data/authors/me.yaml` |
| 网站标题/描述 | `config/_default/params.yaml` |
| 导航菜单 | `config/_default/menus.yaml` |
| 首页布局 | `content/_index.md` |
| 经历页面 | `content/experience.md` + `data/authors/me.yaml` 中的 education/work |
| 论文 | `content/publications/论文名/index.md` |
