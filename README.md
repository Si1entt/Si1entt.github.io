# Luoming Hu's Academic Homepage | 胡洛铭个人学术主页

Personal academic website of **Luoming Hu (胡洛铭)** — Senior B.S. student in Artificial Intelligence at **Dalian University of Technology (DUT)** and incoming Fall 2027 Direct-Ph.D. student at the **[Data Intelligence and Social Computing Lab (Fudan DISC)](http://www.fudan-disc.com/)**, School of Data Science, **Fudan University**, advised by **[Prof. Zhongyu Wei](http://www.fudan-disc.com/people/zywei)**.

- **Live Website | 在线主页**: [https://si1entt.github.io/](https://si1entt.github.io/)

本仓库为 **胡洛铭（Luoming Hu）** 的个人学术主页源码（基于 Jekyll 构建，部署于 GitHub Pages 根域名 [`https://si1entt.github.io/`](https://si1entt.github.io/)），涵盖个人简介、教育与科研经历、代表性论文（Publications）、学术动态（News）以及研究随笔（Blog）。

---

## 1. Directory Structure | 目录结构

```text
academic-homepage/
├── _config.yml                 # Global Jekyll configuration (baseurl, timezone, collections)
├── _data/
│   ├── profile.yml             # Bio, avatar, contact links, education, research experience & honors
│   ├── authors.yml             # Co-author metadata (bolding & external homepage links)
│   ├── display.yml             # Homepage module visibility, news count limit & footer text
│   └── navigation.yml          # Top navigation bar tabs (Home, Publications, Blog, optional Showcase)
├── _publications/              # Paper entries organized by year (e.g., 2025/, 2026/)
├── _news/                      # Concise academic timeline updates shown on the homepage
├── _posts/                     # Academic blog posts & research notes (KaTeX + Prism.js + auto-TOC)
├── _showcase/                  # Optional Masonry cards (Research Highlights, Honors & Interests)
├── _includes/ & _layouts/      # HTML templates and reusable UI widgets
├── assets/
│   ├── css/ & js/              # Stylesheets and scripts (lazy loading, TOC, visual hash, citations)
│   └── images/
│       ├── photos/             # Profile avatars: portrait.jpg (anime) & portrait_formal.jpg (formal)
│       ├── covers/             # Paper overview thumbnails for publication cards
│       └── badges/             # University / institution emblem icons
├── index.html                  # Main two-column homepage
├── publications.html           # Full publications list grouped by year
├── blog.html                   # Blog index grouped by year
└── showcase.html               # Optional showcase gallery page
```

---

## 2. Common Customization Guide | 常用维护指南

### 2.1 Switching Avatar: Anime vs. Formal Portrait | 切换头像（二次元头像 / 正式证件照）

Two profile avatars are stored in `assets/images/photos/`:
- `portrait.jpg`: Custom square anime avatar (**active by default** / 默认启用).
- `portrait_formal.jpg`: Cropped white-background formal portrait (白底正式证件照).

To switch between them, edit `portrait_url` in `_data/profile.yml`:

```yaml
# Option A: Anime avatar (default)
portrait_url: /assets/images/photos/portrait.jpg
# portrait_url: /assets/images/photos/portrait_formal.jpg

# Option B: Formal studio portrait
# portrait_url: /assets/images/photos/portrait.jpg
portrait_url: /assets/images/photos/portrait_formal.jpg
```

> **Note**: Keep `portrait_caption` commented out unless you explicitly want a hover caption under the avatar.

### 2.2 Linking a Public-Safe CV PDF | 配置公开脱敏版简历 PDF

`cv_link` in `_data/profile.yml` is commented out by default to protect personal privacy. To publish a downloadable CV:

1. **Sanitize the PDF first (重要隐私提醒)**: Before placing any CV PDF into this public repository, ensure all sensitive personal information—such as **mobile phone numbers**, **student IDs**, **national ID numbers**, and **residential street addresses**—has been completely removed (retain only public email `huluoming2004@163.com` / `huluoming2004@mail.dlut.edu.cn` and GitHub `Si1entt`).
2. Save the sanitized PDF under `assets/pdf/` (for example, `<sanitized_cv>.pdf`).
3. Uncomment `cv_link` in `_data/profile.yml`:

```yaml
cv_link: /assets/pdf/<sanitized_cv>.pdf
```

### 2.3 Adding Publications, News & Blog Posts | 新增论文、动态与博客

#### Adding a Publication (`_publications/`) | 新增论文
1. Save a clean overview figure (recommended aspect ratio ~`3:2`, white background) to `assets/images/covers/<paper_slug>.png`. (If `cover` is omitted, a deterministic SVG bubble graphic is rendered automatically.)
2. Add any new co-authors with public homepages to `_data/authors.yml` (`"Luoming Hu"` is configured with `bold: true`; append `*` for equal contribution and `#` for corresponding author in the paper's `authors` list).
3. Create `_publications/<YEAR>/<YEAR>-<slug>.md`:

```yaml
---
title:          "Paper Title"
date:           2026-09-26 10:00:00 +0800
selected:       true
pub:            "Advances in Neural Information Processing Systems (NeurIPS)"
pub_date:       "2026"
pub_last:       ' <span class="badge badge-pill badge-publication badge-success">NeurIPS 2026</span>'
abstract: >-
  Concise 1–2 sentence TL;DR summary of the core problem, method, and empirical result.
cover:          /assets/images/covers/<paper_slug>.png
authors:
  - Luoming Hu*
  - Coauthor Name#
links:
  Paper: https://arxiv.org/abs/xxxx.xxxxx
  Code: https://github.com/Si1entt
---
```

#### Adding a News Item (`_news/`) | 新增学术动态
Create `_news/YYYY-MM-DD-slug.md` (ensure `date` is not in the future relative to build time so Jekyll renders it immediately):

```yaml
---
title: >-
    One paper is accepted to <strong>ACL 2027</strong>!
    <a href="https://aclanthology.org/" target="_blank">[Paper]</a>
date: 2026-11-20 10:00:00 +0800
---
```

#### Writing a Blog Post (`_posts/`) | 撰写学术博客
Create `_posts/YYYY-MM-DD-slug.md`. The `blog_post` layout automatically builds a sticky right-hand Table of Contents from `##` / `###` headings and supports inline (`$...$`) and display (`$$...$$`) KaTeX math as well as Prism.js syntax highlighting:

```yaml
---
layout: blog_post
title: "Post Title"
date: 2026-11-20 14:00:00 +0800
tags:
  - LLM Alignment
  - Mechanistic Interpretability
excerpt: >-
  Short summary displayed on the /blog index card.
---
```

> **Math Tip (KaTeX + Kramdown)**: Wrap standalone display equations in a block HTML `<div>` (e.g., `<div class="my-3 text-center">$$...$$</div>`) and escape underscores as `\_` inside inline `$...$` math so Kramdown passes KaTeX delimiters through untouched.

*(Optional)* To display the **Showcase** page (`_showcase/projects/`) in the top navbar, uncomment the `Showcase` block in `_data/navigation.yml`.

---

## 3. Deployment & `baseurl` Configuration | 部署与 `baseurl` 配置

### GitHub Pages `baseurl` Settings (`_config.yml`)

This site is actively deployed as a **GitHub Pages User Root Site** at [`https://si1entt.github.io/`](https://si1entt.github.io/) (repository [`Si1entt/Si1entt.github.io`](https://github.com/Si1entt/Si1entt.github.io)) with `url: "https://si1entt.github.io"` and `baseurl: ""` in `_config.yml`:

| Deployment Mode | Repository Name | Published URL | `_config.yml` Setting | Status |
| :--- | :--- | :--- | :--- | :--- |
| **User Root Site (个人主域名仓库)** | `Si1entt.github.io` | `https://si1entt.github.io/` | `baseurl: ""` | **Active (当前启用)** |
| **Project Subpath Site (项目子路径仓库)** | `academic-homepage` | `https://si1entt.github.io/academic-homepage/` | `baseurl: "/academic-homepage"` | Optional |

> **Tip**: Pushing commits to the `main` branch of `Si1entt/Si1entt.github.io` automatically triggers GitHub Pages to rebuild and publish the site. The homepage also includes a built-in URL check widget that alerts you if `baseurl` ever diverges from your deployed repository path.

### Local Preview | 本地预览

If Ruby and Bundler are installed locally:

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000/` in your browser.

---

## Acknowledgements | 致谢

This website is built upon the clean and responsive [**academic-homepage**](https://github.com/luost26/academic-homepage) Jekyll template created by [Shitong Luo](https://luost.me/), licensed under the [MIT License](LICENSE).
