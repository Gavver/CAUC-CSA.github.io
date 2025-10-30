# 编写指南

---


- mkdocs对markdown解析并不能十全十美，提交前请务必自行构建确认页面完整无误。
- 本项目基于 mkdocs-material 项目结构请查阅[mkdocs官方文档](https://mkdocs.org.cn/)和[mkdocs-material官方文档](https://squidfunk.github.io/mkdocs-material/)，请务必首先阅读文档再进行提交。
- 本项目文档均在 docs 目录下编写，编写完成后push至main/master分支将自动更新页面。
- 对于组件使用，可以依据本项目已有文章中的格式进行编写。

## 编写步骤

1. 确保python安装（建议3.10及以上）

2. 如果你会使用虚拟环境请创建并激活虚拟环境，否则可跳过此步骤

3. 安装依赖库
    ```bash
    pip install -r requirements.txt
    ```
4. 在mkdocs.yml最下方nav目录适当位置加入新页面，在docs目录下创建对应markdown文件

5. 本地构建运行
    ```bash
    mkdocs serve --livereload
    ```

6. 浏览器访问 http://127.0.0.1:8000/

## 一些建议

1. 对于内链使用`[]()`格式即可，对于希望新标签页打开的链接则需在链接后添加`{target="_blank" rel="noopener"}`，例如：
    ```markdown
    [点击这里在新窗口中打开链接](https://example.com){target="_blank" rel="noopener"}
    ```
    from N0rth5ea in 2025-10-28
2. 本页跳转时，锚点的使用和md不一致，建议本地运行后点击锚点，通过url的显示设置锚点。
    </br>例如：url为`http://127.0.0.1:8000/contributing/writing/#_3`
    </br>本页面为`docs/contributing/writing.md`，锚点为`#_3`，则对应跳转声明是：
    ```markdown
    [跳转到3. 编写步骤](#_3)
    ```
    忽略pycharm等报错`无法解析定位标记 _3`，因为mkdocs对锚点的处理与md不一致，实际跳转是可行的。
    </br>from N0rth5ea in 2025-10-28
3. 有些md编辑器会导致缩进为3个空格，导致解析异常，如果发现页面显示不理想可以检查是否缩进问题。（目前在构建时已会自动修正三格缩进）
    </br>也可以使用正则表达式替换所有3个空格为4个空格。（推荐工具Notepad3）
    ```regex
    ^\s{3}(?!\s)
    ```
    </br>from N0rth5ea in 2025-10-28
