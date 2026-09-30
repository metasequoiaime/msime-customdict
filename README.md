# 水杉输入法共享自定义词库

通用词典给不全的词，由人工维护在这里。[msime](https://github.com/metasequoiaime/msime) 与 [MSIME-Windows](https://github.com/metasequoiaime/MSIME-Windows) 共用这一份：合入的条目会在同一次词库更新里同时出现在所有平台上。

## 目录

```
data/
  words.txt          中文词条，并入全拼词表
  translations.txt   候选窗翻译覆盖
  english.txt        英文词条（目前没有构建读取）
  names.txt          人名（目前没有构建读取）
packs/               用户按需导入的专业词库，不进入默认词库
scripts/             维护脚本
```

## 格式

| 文件 | 每行 | 说明 |
| --- | --- | --- |
| `data/words.txt` | `词<TAB>全拼<TAB>权重` | 全拼音节用 `'` 分隔，如 `未来可期	wei'lai'ke'qi	5000`。已有同词同拼音时只会调高权重 |
| `data/translations.txt` | `源词<TAB>译文` | 优先于 ECDICT；源词含汉字为中译英，否则为英译中；`#` 开头为注释，同一源词后写覆盖先写 |
| `data/english.txt` | `编码<TAB>显示词<TAB>权重` | |
| `data/names.txt` | 一个人名 | |

## 贡献

- **官网提交**：在 [msime.app](https://msime.app) 的词条提交页填写，条目会追加到本仓库一个滚动的 Pull Request。
- **直接提 Pull Request**：往 `data/words.txt` 末尾追加行即可，不要改动已有的行。

每个改动 `data/words.txt` 的 Pull Request 都会由 CI 用词库构建器的同一套规则检查：只能追加、格式与拼音合法、权重在现有范围内、不与本次改动、现有词表或已发布词库重复。结果以评论写在 Pull Request 里。维护者再人工审核内容（包括敏感词）后合入。

## 怎样进入输入法

1. 条目合入本仓库。
2. msime 在 `resources/dictionary-sources.lock.json` 里把本仓库的固定提交升到新版本，用 `msime-dict-build` 构建并发布 `dict-v*` 词库。
3. msime 与 MSIME-Windows 升级各自锁定的词库版本。

## 专业词库

| 词库 | 内容 |
| --- | --- |
| [`packs/unreal_houdini`](packs/unreal_houdini) | Unreal Engine、Houdini 和 Houdini Engine for Unreal |

专业词库不会进入所有人的候选列表，用户在设置里导入后才作为用户词条生效，所以垂直领域的短缩写（`SOP`、`TOP`、`PCG`）不会干扰普通输入。检查格式、拼音、重复项和翻译覆盖率：

```sh
python3 scripts/validate_packs.py
```

## 历史

这些文件在 2026 年 9 月曾并入 MSIME-Engine 的 `dictionary/custom/`，Engine 被 msime 的 Rust 引擎取代后回到这里，其间的修改已同步回来。
