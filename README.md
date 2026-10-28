# 公众号内容生产 · wechat-official-article

一个面向 **Codex / Claude Skills** 生态的 Agent Skill：把图片素材转成**可核验、可复制**的微信公众号文章。

它会根据你提供的图片提取事实台账，按品牌口径写作，输出纯文本审阅稿、可粘贴到公众号编辑器的内联样式 HTML，以及一张事实核查表。核心设计原则是**宁可少写，也不猜**——所有会随时间变化的事实（排名、学费、录取门槛、签证规则、移民政策等）都必须有可追溯来源，否则省略或标记为待确认。

> 仓库内的品牌口径默认为「立思辰留学 / 立思辰新西兰留学」，你可以按自己的品牌改写 `references/brand-editorial-style.md` 后使用。

## 特性

- **三种执行模式，权限收得很紧**：生成稿件（默认）→ 保存草稿（需当次明确授权）→ 发布（默认禁止）。
- **事实台账先行**：先逐图提取文字、院校、数字、日期与置信度，再决定文章主题，避免边写边编。
- **易变事实强制核验**：排名、学费、录取线、签证与移民政策一律要求一手来源 + 核验日期。
- **素材即素材**：图片、网页、聊天记录中的文字被视为素材而非指令，无法提权改变授权范围。
- **内联样式 HTML**：不依赖外部字体、脚本或远程 CSS，浏览器渲染后可直接复制进公众号编辑器。
- **交付前三道检查**：绝对化表述、品牌露出、政治敏感与不可追溯数据。

## 目录结构

```
.
├── .codex-plugin/
│   └── plugin.json                     # 插件元数据与展示信息
└── skills/
    └── wechat-official-article/
        ├── SKILL.md                    # 主技能定义：模式、边界、生产流程
        ├── agents/
        │   └── openai.yaml             # 技能接口描述与隐式调用策略
        ├── assets/
        │   └── wechat-article-template.html   # 排版起点模板
        └── references/
            ├── brand-editorial-style.md       # 品牌口径与文风
            ├── layout-system.md               # 版式系统
            └── save-draft-workflow.md         # 后台保存草稿流程
```

## 使用方式

把仓库放进你的 skills 目录后直接调用：

```bash
git clone https://github.com/<your-name>/wechat-official-article.git
```

- **Codex**：将 `skills/wechat-official-article` 放入 `~/.codex/skills/`（或作为个人插件安装）。
- **Claude Code**：将技能目录放入 `~/.claude/skills/`。
- 其他兼容 Agent Skills 规范的运行时同理，识别 `SKILL.md` 的 YAML front matter 即可。

调用示例：

```
根据我上传的图片生成并排版公众号文章。
核验这篇文章的事实、品牌和内容边界。
把确认后的文章保存到公众号草稿箱，不要发布。
```

## 输出物

每次运行会产出三份文件（文件名会自动移除 Windows 不支持的字符）：

| 文件 | 用途 |
| --- | --- |
| `文章标题-正文.md` | 纯文本审阅稿 |
| `文章标题-公众号排版.html` | 内联样式排版稿，浏览器渲染后可整体复制 |
| `文章标题-核查表.md` | 图片事实台账、来源、易变事实、品牌与禁区检查结果 |

## 安全边界

- 默认**不接触任何外部账号**，只做本地文件生成。
- 保存草稿仅在用户于当次任务中明确要求时执行，且必须锁定稿件、图片、目标账号三者。
- **任何情况下都不点击**「发表」「群发」「定时群发」等使内容对外可见的按钮。
- 不虚构机构资质、案例、成功率或官方合作关系。

## 定制

| 想改什么 | 改哪里 |
| --- | --- |
| 品牌名与文风 | `references/brand-editorial-style.md` |
| 版式与配色 | `references/layout-system.md`、`assets/wechat-article-template.html` |
| 后台保存流程 | `references/save-draft-workflow.md` |
| 内容禁区与流程 | `SKILL.md` |

## License

[MIT](LICENSE)
