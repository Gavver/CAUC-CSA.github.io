# 项目使用方法

---

## 适用人群

## 使用方法

## 贡献方法

提醒：

- 请首先学习git基础知识。
- 本项目基于 mkdocs-material 项目结构请查阅[官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后push至main/master分支将自动更新页面。

步骤：

1. 首次克隆`git clone https://github.com/CAUC-CSA/CAUC-CSA.github.io.git`

2. 每次添加前获取最新主分支，并新建工作分支

   ```bash
   git checkout main 
   git pull origin main
   git checkout -b <your-work-branch>
   ```

3. 在docs下编写文档...

4. 提交更改（其中git commit -m 只能写标题 不加-m的话就能进编译器写 commit title 和 commit message）

   ```bash
   git add .
   git commit -m "更新文档描述"
   git push origin <your-work-branch>
   ```

5. 在 GitHub 网站创建 Pull Request
