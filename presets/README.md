# 职业预设内容

当前包含律师／法务、大学生的 5 个代表岗位和 13 个去重技能，每个岗位绑定 3 个技能。入口内容保留固定上游工作流程，配套资源随技能分发，中文岗位指令映射到 App 授权工具和任务目录。

`catalog.json` 是准备好的内容元数据；`readiness.json` 记录内容审阅与运行验收状态。未完成真实模型运行验收的岗位不进入正式下载目录。法律技能围绕接待和资料整理，不提供未经核验的中国程序模板或期限计算。

当前 5 个岗位已在 macOS arm64、Qoder／Qwen3.8-Max 上完成代表性模拟资料任务，实际读取配套技能并交付 Markdown 成果。状态绑定岗位清单及全部技能摘要；其他模型、平台和 HTML 等额外交付格式仍需对应验收。运行明细与成果由发布者单独保存，不随技能包分发。

## 本地验收包

```sh
python3 scripts/presets/build.py --output /tmp/multi-ai-presets
```

生成 ZIP 可在智能体市场本地导入。不会把未验收包加入官方目录。资源摘要由排序后的 `[相对路径, 文件 SHA256]` JSON 行计算，与 Core 安装器一致。

## 验收与发布

使用 `tests/fixtures/presets` 的模拟输入执行真实成员任务，逐个记录实际模型、平台、输入、成果与检查结果。验收报告每个岗位包含 `version`、`contentReview: passed`、`runtimeAcceptance: passed`、`presetDigest`（由打包模块 `preset_digest(preset)` 计算）、`skillDigests`（技能 key → 当前摘要）及 `evidence`（实际验收记录路径或说明）。岗位指令与技能内容均须匹配验收摘要。通过验收后再生成发布目录：

```sh
python3 scripts/presets/build.py --output /tmp/multi-ai-presets-release \
  --repository OWNER/REPOSITORY --tag presets-1.0.0 --acceptance /path/to/acceptance.json
```

在维护方确认的 GitHub 仓库创建固定版本 Release，上传 ZIP 与 `catalog.json`，将目录 HTTPS 地址作为 App 的官方发布配置。构建工具只打包，不创建或发布 Release。每次技能资源修改必须更新摘要并重新验收；后续发布使用新版本与新标签。

运行开发版时设置 `MULTI_AI_MARKET_CATALOG_URL`；执行 `pnpm package:mac` 或 `pnpm package:win` 时同样设置此环境变量，打包过程会将地址保存为随 App 分发的 `market-config.json`。未设置时，本地导入仍可用。此地址只包含公开的 HTTPS 目录位置，不含账号凭据。

验收要求：

| 岗位 | 代表场景 | 检查结果 |
|---|---|---|
| 案件接待助理 | 模拟合同、邮件和接待陈述 | 清楚区分已知、陈述、待核验；未捏造付款证据、法域与程序期限 |
| 证据整理助理 | 两个来源的初稿日期有差异 | 同时保留日期和原文定位；字段表与来源表一致；缺口不填补 |
| 课程辅导助教 | 学生混淆两个条件概率 | 先识别学生尝试，提示分层；推导保留非零分母与零交集条件；不虚构学生已掌握 |
| 复习备考教练 | 14 天、每天 25 分钟 | 练习依据课程材料，答案分开，安排可执行，并根据错误调整 |
| 实习简历顾问 | 项目材料缺少数字与实习经历 | 不补造经历或百分比；岗位匹配可追溯，逐项记录定制；生成 HTML 时另行核对其内容与结构化资料一致 |

## 授权与来源

每个技能附 `LICENSE` 和 `NOTICE`；元数据记录源仓库、40 位固定提交、原始路径、Star 核验和日期。教育技能及其改动按 CC BY-SA 4.0 分发，案件技能按 Apache 2.0、简历技能按 MIT 分发。许可范围只覆盖相应材料，不影响应用其他代码的授权。

增加岗位时优先复用已适配技能，仍需按具体职责验收。其余职业入口由 App 统一分类提供，上架内容通过官方目录逐步增加。
