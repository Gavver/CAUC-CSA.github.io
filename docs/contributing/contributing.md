# 贡献指南

---

本文将说明如何提交无论是新文档还是修改至wiki页面，在这之前你应该

- 请首先学习git基础知识。
- 本项目基于 mkdocs-material 项目结构请查阅[mkdocs官方文档](https://mkdocs.org.cn/)和[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后push至main/master分支将自动更新页面。
- 对于组件使用，可以依据本项目已有文章中的格式进行编写。

## 提交步骤

1. 首次克隆
   ```bash
   git clone https://github.com/CAUC-CSA/CAUC-CSA.github.io.git
   ```

2. 每次添加前获取最新主分支，并新建工作分支
   ```bash
   git checkout main 
   git pull origin main
   git checkout -b <your-work-branch>
   ```

3. 在docs下编写文档，强烈建议在本地构建，确认页面无误后再提交，请阅读[编写指南](writing.md)

4. 提交更改（其中git commit -m 只能写标题 不加-m的话就能进编译器写 commit title 和 commit message）
   ```bash
   git add .
   git commit -m "更新文档描述"
   ```

5. 同步最新代码
   ```bash
   git checkout main
   git pull origin main
   git checkout dev
   git merge main
   ```
   解决冲突（如果有的话）

6. 推送至远程仓库
   ```bash
   git push origin <your-work-branch>
   ```

7. 在 GitHub 网站创建 Pull Request