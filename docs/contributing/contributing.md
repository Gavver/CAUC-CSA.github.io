# 贡献指南

---

!!! warning "注意"
    禁止直接push至 main/master 分支，所有更改必须通过 Pull Request 方式提交，经审核通过后方可合并。

本文将说明如何提交无论是新文档还是修改至wiki页面，在这之前你应该

- 请首先学习git基础知识。
- 本项目基于 mkdocs-material 项目结构请查阅[mkdocs官方文档](https://mkdocs.org.cn/)和[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后推送至 master 分支，CI 将自动编译并更新页面。
- 对于组件使用，可以依据本项目已有文章中的格式进行编写。

## 提交步骤

### 方式一：直接克隆（仅限有仓库写权限的成员）

1. 首次克隆
   ```bash
   git clone https://github.com/CAUC-CSA/CAUC-CSA.github.io.git
   ```

2. 每次添加前获取最新主分支，并新建工作分支
   ```bash
   git checkout master 
   git pull origin master
   git checkout -b <your-work-branch>
   ```

3. 在docs下编写文档，强烈建议在本地构建，确认页面无误后再提交，请阅读[编写指南](writing.md)

4. 提交更改（其中 git commit -m 只能写 commit message，不加 -m 的话就能进编辑器写 commit message 和 extended description）
   ```bash
   git add .
   git commit -m "更新文档描述"
   ```

5. 同步最新代码（建议使用 rebase 保持线性历史）
   ```bash
   git fetch origin
   git rebase origin/master
   ```
   如果遇到冲突，解决后执行：
   ```bash
   git add .
   git rebase --continue
   ```

6. 推送至远程仓库
   ```bash
   # 首次推送
   git push origin <your-work-branch>
   
   # 如果 rebase 后再次推送，需要使用
   git push --force-with-lease origin <your-work-branch>
   ```

7. 在 GitHub 网站创建 Pull Request

### 方式二：Fork 后提交

1. 在 GitHub 上 Fork 本仓库到你的账号下

2. 克隆你 fork 的仓库
   ```bash
   git clone https://github.com/<your-username>/CAUC-CSA.github.io.git
   cd CAUC-CSA.github.io
   ```

3. 添加上游仓库
   ```bash
   git remote add upstream https://github.com/CAUC-CSA/CAUC-CSA.github.io.git
   ```

4. 创建工作分支
   ```bash
   git checkout -b <your-work-branch>
   ```

5. 在docs下编写文档，强烈建议在本地构建，确认页面无误后再提交，请阅读[编写指南](writing.md)

6. 提交更改
   ```bash
   git add .
   git commit -m "更新文档描述"
   ```

7. 同步上游仓库最新代码
   ```bash
   git fetch upstream
   git rebase upstream/master
   ```
   如果遇到冲突，解决后执行：
   ```bash
   git add .
   git rebase --continue
   ```

8. 推送到你的 fork 仓库
   ```bash
   # 首次推送
   git push origin <your-work-branch>
   
   # 如果 rebase 后再次推送，需要使用
   git push --force-with-lease origin <your-work-branch>
   ```

9. 在 GitHub 上从你的 fork 仓库创建 Pull Request 到主仓库
