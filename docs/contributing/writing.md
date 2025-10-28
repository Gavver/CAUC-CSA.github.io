# 编写指南

---


- mkdocs对markdown解析并不能十全十美，提交前请务必自行构建确认页面完整无误。
- 本项目基于 mkdocs-material 项目结构请查阅[mkdocs官方文档](https://mkdocs.org.cn/)和[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后push至main/master分支将自动更新页面。
- 对于组件使用，可以依据本项目已有文章中的格式进行编写。

## 编写步骤

1. 确保python安装（建议3.10及以上）

2. 安装依赖库
    ```bash
    pip install -r requirements.txt
    ```
3. 在mkdocs.yml最下方nav目录适当位置加入新页面，在docs目录下创建对应markdown文件

4. 本地构建运行
   ```bash
   mkdocs serve
   ```

5. 浏览器访问 http://127.0.0.1:8000/

## 一些建议

1. 对于内链使用`[]()`格式即可，对于希望新标签页打开的链接则需在链接后添加`{target="_blank"}`:
   ```markdown
   [点击这里在新窗口中打开链接](https://example.com){target="_blank"}
   ```
   from N0rth5ea in 2025-10-28