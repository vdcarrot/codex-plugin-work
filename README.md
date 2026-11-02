# 个人 Codex 工作插件

维护工作中使用的 skills，通过仓库插件源按需安装。每个插件自包含，可单独使用。

| 插件 | Skill | 用途 | 触发方式 |
| --- | --- | --- | --- |
| `project-analysis` | `extract-business-facts` | 从整项目、单功能或一组功能的代码中提取可追溯业务事实 | 明确的业务理解任务可自动匹配，也可点名使用 |
| `information-design` | `organize-complex-information` | 将 PRD、业务资料等复杂信息组织为由整体到细节的多层结构 | 仅用户明确点名使用 |

## 添加插件源

在仓库根目录运行：

```sh
codex plugin marketplace add .
codex plugin marketplace list
```

也可通过 `codex plugin marketplace add owner/repo` 注册托管后的 Git 仓库。打开 Codex 插件目录，选择 `codex-plugin-work` 源并安装所需插件，再在新聊天中使用。更新源时可运行 `codex plugin marketplace upgrade codex-plugin-work`，并通过客户端支持的流程刷新已安装插件。

## 使用示例

```text
使用 $extract-business-facts 分析这个仓库的业务，先建立全景，再核实主要能力与规则。
```

```text
使用 $extract-business-facts 核实申请提交、材料更新和审批这组功能。
重点说明确认何时有效、哪些变化会使它失效，并给出代码证据。
```

```text
使用 $organize-complex-information 重新组织这份 PRD，面向首次接触项目的开发者。
从整体目标和关系逐步深入，每个信息块都保留其全局作用及必要条件。
```

两个 skill 没有调用依赖。需要组合时，在请求中分别点名；单纯要求阅读长文、总结或分析代码，不会因此自动启用信息重组 skill。它的 `agents/openai.yaml` 设置了 `policy.allow_implicit_invocation: false`，描述和正文也限定显式调用。

## 仓库结构与验证

仓库源清单位于 `.agents/plugins/marketplace.json`，路径相对于仓库根目录。插件的 `plugin.json`、`skills/` 与 `tests/` 均位于对应插件根目录。每个 skill 的必要流程保留在 `SKILL.md`，记录模板和示例按需读取。

`tests/<skill-name>.md` 保存正向、反向与边界场景，供独立试用和回归检查；格式校验不能替代行为验证。贡献约定见 [AGENTS.md](./AGENTS.md)。格式依据：[OpenAI 插件打包规范](https://developers.openai.com/plugins/build/plugins)、[Skill 元数据规范](https://developers.openai.com/plugins/deploy/submission-errors)。
