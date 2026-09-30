# 水杉输入法自定义词库

这个仓库保存无法从通用词典稳定取得的人工维护条目，是水杉输入法各平台共用的那一份：[msime](https://github.com/metasequoiaime/msime) 的词库构建器 `msime-dict-build` 按固定提交读取这里的文件，构建出的词库以 `dict-v*` 发行版发布，msime 与 [MSIME-Windows](https://github.com/metasequoiaime/MSIME-Windows) 都锁定到同一个发行版，所以一条词条合入这里之后，两边在同一次词库更新中一起生效。

## 文件

| 文件 | 格式 | 用途 |
| --- | --- | --- |
| `words.txt` | `词<TAB>全拼<TAB>权重`，全拼音节用 `'` 分隔 | 并入全拼词表；已有同词同拼音的条目时只会调高权重，不会调低 |
| `translations.txt` | `源词<TAB>译文`，`#` 开头为注释 | 候选窗翻译覆盖，优先于 ECDICT 大词库；源词含汉字为中译英，否则为英译中，同一源词后写覆盖先写 |
| `english.txt` | `编码<TAB>显示词<TAB>权重` | 英文词条，目前没有构建读取 |
| `names.txt` | 每行一个人名 | 人名，目前没有构建读取 |
| `packs/` | 见各词库自己的 README | 用户在设置里按需导入的专业词库，不进入默认词库 |

## 改动怎么进入产品

1. 在这里提 Pull Request 修改上面的文件。
2. CI 检查格式、拼音和重复项，并在 Pull Request 里列出新增与被拒绝的条目及原因。
3. 维护者审核（尤其是敏感词）后合入。
4. msime 在 `resources/dictionary-sources.lock.json` 里把本仓库的固定提交升到新版本，重新构建并发布 `dict-v*` 词库。
5. msime 与 MSIME-Windows 升级各自锁定的词库版本。

官网的词条提交入口会把用户提交的条目追加到一个滚动 Pull Request，同样经过上面的检查与审核。

## 专业词库

- [`packs/unreal_houdini`](packs/unreal_houdini)：Unreal Engine、Houdini 和 Houdini Engine for Unreal

专业词库不会自动加入所有用户的候选列表。用户选择导入后，新增内容会作为用户词条保存，因此既能满足垂直领域输入，也不会用 `SOP`、`TOP`、`PCG` 等短缩写干扰普通用户。检查所有专业词库的字段格式、拼音、重复项和翻译覆盖率：

```sh
python tools/validate_packs.py
```

## 历史

这些文件曾在 2026-09 并入 MSIME-Engine 的 `dictionary/custom/`（本仓库当时被归档），Engine 由 msime 的 Rust 引擎取代后，这里重新成为唯一的来源。Engine 期间的修改已同步回来，完整提交历史保留在两边。
