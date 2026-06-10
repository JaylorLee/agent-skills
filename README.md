# jaylor-skills

我的个人 **跨工具 skill 库**。单一真相源:所有 skill 都放在这里,Claude Code、Codex 等工具各自指向本仓库,**只在一处维护**。

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

## 新增一个 skill

1. 在 `plugins/my-skills/skills/` 下新建目录,例如 `.../skills/my-new-skill/`
2. 写该目录下的 `SKILL.md`,frontmatter 至少包含:
   ```markdown
   ---
   name: my-new-skill
   description: 一句话说明什么时候该用它(触发判断靠这句)
   ---
   ```
3. 更新各工具缓存:
   ```bash
   claude plugin marketplace update jaylor-skills
   codex  plugin marketplace update jaylor-skills   # 或重新 add
   ```

## 在各工具中启用

```bash
# Claude Code
claude plugin marketplace add ~/skills
claude plugin install my-skills@jaylor-skills

# Codex
codex plugin marketplace add ~/skills
codex plugin add my-skills
```
