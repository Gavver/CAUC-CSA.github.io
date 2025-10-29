# 贡献指南

---

!!! warning "注意"
    禁止直接 push 至 master 分支，所有更改必须通过 Pull Request 方式提交，经审核通过后方可合并。

本文将说明如何提交无论是新文档还是修改至 Wiki 页面，在这之前你应该

- 请首先学习 git 基础知识。
- 本项目基于 mkdocs-material，项目结构请查阅[mkdocs官方文档](https://mkdocs.org.cn/)和[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后通过 Pull Request 合并至 master 分支，CI 将自动编译并更新页面。
- 对于组件使用，可以依据本项目已有文章中的格式进行编写。

## 提交步骤

### 方式一：直接克隆（仅限有仓库写权限的成员）

1. 首次克隆仓库

    ```bash
    git clone https://github.com/CAUC-CSA/CAUC-CSA.github.io.git
    ```

2. 获取最新主分支（首次克隆后立刻开始工作可跳过此步骤，仅在每次开始新工作前执行）

    ```bash
    git checkout master
    git pull origin master
    ```

3. 创建新的工作分支

    ```bash
    git checkout -b <your-work-branch>
    ```

4. 在 docs 下编写文档，强烈建议在本地构建，确认页面无误后再提交，请阅读[编写指南](writing.md)

5. 提交更改

    ```bash
    git add .
    git commit -m "对你作出的更新的描述"
    ```

    > `git commit -m` 可以直接写提交说明（commit message），包括主题和正文；不加 `-m` 会进入编辑器，可以更详细地编写多行提交说明，包括主题和扩展描述

6. 推送至远程仓库

    ```bash
    git push origin <your-work-branch>
    ```

7. 在 GitHub 网站创建 Pull Request

8. 若 PR 没有问题，已合并入主分支，那么就可以删除自己本地的分支了

    ```bash
    git checkout master
    git branch -D <your-work-branch>
    ```

9. 若后续需要增加新的提交，首先拉取远程仓库的最新代码

    ```bash
    # 此时已经在 master 分支
    git pull origin master
    ```

10. 重复步骤 3~7（创建新分支 → 编写文档 → 提交 → 推送 → 创建 PR 等待 review）即可

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

5. 在 docs 下编写文档，强烈建议在本地构建，确认页面无误后再提交，请阅读[编写指南](writing.md)

6. 提交更改

    ```bash
    git add .
    git commit -m "对你作出的更新的描述"
    ```

7. 推送到你的 fork 仓库

    ```bash
    git push origin <your-work-branch>
    ```

8. 在 GitHub 上从你的 fork 仓库创建 Pull Request 到主仓库

9. 若 PR 没有问题，已合并入主分支，那么就可以删除自己本地的分支了

    ```bash
    git checkout master
    git branch -D <your-work-branch>
    ```

10. 若后续需要增加新的提交，首先同步上游仓库的最新代码

    ```bash
    # 此时已经在 master 分支
    git pull upstream master
    git push origin master  # 同步到你的 fork 仓库
    ```

    或者直接在 GitHub 网页上通过 "Sync fork" 按钮同步，然后拉取：

    ```bash
    # 此时已经在 master 分支
    git pull origin master
    ```

11. 然后重复步骤 4~8（创建新分支 → 编写文档 → 提交 → 推送 → 创建 PR 等待 review）即可
