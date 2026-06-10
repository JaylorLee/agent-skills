# jaylor-skills

我的个人 **跨工具 skill 库**。单一真相源:所有 skill 都放在这里,Claude Code、Codex 等工具各自指向本仓库,**只在一处维护**。

- GitHub:<https://github.com/JaylorLee/agent-skills>
- 市场名:`jaylor-skills`,插件:`my-skills`

## 结构

```
.claude-plugin/
  marketplace.json              # 声明本仓库为一个插件市场,内含 my-skills 插件
plugins/
  my-skills/
    .claude-plugin/plugin.json  # my-skills 插件元数据
    skills/
      <skill-name>/
        SKILL.md                # 每个 skill 的定义(frontmatter: name + description)
AGENTS.md                       # 给 Codex / cursor-agent 等读 AGENTS.md 约定的工具
```

> 结构遵循官方插件约定(每个插件在 `plugins/<名字>/`,`source` 指向它),
> 这样 Claude 和 Codex 都能解析。

## 首次在一台机器上启用

工具拉的是 **GitHub git 源**(不是本地路径),所以任何装了 `claude` / `codex` 的机器都能用:

```bash
# Claude Code
claude plugin marketplace add JaylorLee/agent-skills
claude plugin install my-skills@jaylor-skills

# Codex
codex plugin marketplace add JaylorLee/agent-skills
codex plugin add my-skills@jaylor-skills
```

## 新增 / 修改一个 skill

1. 编辑或新建 `plugins/my-skills/skills/<名字>/SKILL.md`,frontmatter 至少包含:
   ```markdown
   ---
   name: my-new-skill
   description: 一句话说明什么时候该用它(触发判断靠这句)
   ---
   ```
2. **提交并推送**(关键:工具拉的是 GitHub,不 push 不生效):
   ```bash
   git add -A && git commit -m "说明" && git push
   ```
3. 刷新各工具缓存(git 源支持 upgrade):
   ```bash
   claude plugin marketplace update  jaylor-skills
   codex  plugin marketplace upgrade jaylor-skills
   ```
4. 新 skill 需 **新开会话** 才会出现在工具的 skill 列表里。

## 说明

- 本仓库的本地副本(如 `~/skills`)只是**编辑用的工作副本**;工具运行时读取的是 GitHub 上的版本。
- 不要再把个人 skill 直接放进 `~/.claude/skills/`——本仓库是唯一真相源。
