# AI 辅助代码审查完整教程

> 四步流程，结合多个 AI 模型的优势，高效完成代码审查

## 🎯 核心优势

- ⚡ **速度快**：AI 几分钟完成初步审查
- 🎯 **准确性高**：人工确认避免误报
- 🔍 **覆盖全面**：自动发现代码库中的类似问题
- 💰 **成本优化**：根据任务选择合适的 AI 模型

---

## 📦 安装

一条命令完成所有配置：

```bash
uvx --from git+https://github.com/coderdevin/spec-kit specify init . --ai cursor --force
```

---

## 🔄 完整流程

### 步骤 1：AI 初步审查 🤖

**使用模型**：Claude-4.5-thinking（擅长深度分析）

**命令格式**：
```bash
speckit.codereview-<语言> <目标路径> 中文输出
```

**具体示例**：

```bash
# 审查 Python 代码
speckit.codereview-python src/api 中文输出

# 审查 React 代码  
speckit.codereview-react src/components 中文输出

# 审查 Java 代码
speckit.codereview-java src/main/java 中文输出
```

**结果**：生成报告文件 `.specify/reviews/codereview-*.md`

---

### 步骤 2：人工确认 ✅

**这是最关键的一步！**

1. 打开 `.specify/reviews/` 目录中生成的报告文件
2. 仔细阅读 AI 发现的每个问题
3. 在报告的任务列表中，**对真实问题打勾**

**示例**：

```markdown
## 待确认问题清单

- [ ] 问题1：缺少输入验证，可能导致SQL注入
- [ ] 问题2：变量命名不规范
- [ ] 问题3：未处理异常，可能导致崩溃
- [ ] 问题4：循环嵌套过深，影响性能
```

**判断后打勾**（`[x]` 表示确认是真实问题）：

```markdown
- [x] 问题1：缺少输入验证，可能导致SQL注入  ← 确认是问题
- [ ] 问题2：变量命名不规范  ← 这只是风格问题，忽略
- [x] 问题3：未处理异常，可能导致崩溃  ← 确认是问题
- [ ] 问题4：循环嵌套过深，影响性能  ← 这个场景可以接受
```

**打勾标准**：
- ✅ **应该打勾**：安全漏洞、逻辑错误、性能问题、违反最佳实践
- ❌ **不用打勾**：代码风格偏好、误报、不适用的建议

---

### 步骤 3：深度验证 🔬

**使用模型**：GPT-5-Codex（擅长代码分析和模式识别）

**作用**：
- 验证你标记的问题是否真实存在
- 在整个代码库中查找类似的问题
- 一网打尽所有相关问题

**命令**：
```bash
speckit.codereview-check <步骤1生成的报告文件路径>
```

**示例**：
```bash
speckit.codereview-check .specify/reviews/codereview-python-2024-10-29-143052.md
```

**结果**：生成验证报告 `.specify/reviews/validation-*.md`

---

### 步骤 4：生成总结 📊

**使用模型**：Claude-4.5-haiku（文本整理能力强，成本低）

**作用**：整合所有审查结果，生成最终报告

**命令**：
```bash
speckit.codereview-summary <审查报告> <验证报告>
```

**示例**：
```bash
speckit.codereview-summary \
  .specify/reviews/codereview-python-2024-10-29-143052.md \
  .specify/reviews/validation-2024-10-29-150230.md
```

**结果**：生成总结报告 `.specify/reviews/summary-*.md`

这份报告包含：
- 📋 问题总览和严重程度分类
- 🎯 修复优先级和时间估算
- 📝 可以直接分享给团队的完整文档

---

## 💡 完整示例演示

假设你要审查一个 Python API 项目：

```bash
# 第 1 步：AI 初步审查
speckit.codereview-python src/api 中文输出
# ✅ 生成：.specify/reviews/codereview-python-2024-10-29-143052.md

# 第 2 步：人工确认（打开文件，标记问题，保存）
# 编辑器中打开 .specify/reviews/codereview-python-2024-10-29-143052.md
# 在任务列表中对真实问题打勾 [x]
# 保存文件

# 第 3 步：深度验证
speckit.codereview-check .specify/reviews/codereview-python-2024-10-29-143052.md
# ✅ 生成：.specify/reviews/validation-2024-10-29-150230.md

# 第 4 步：生成总结
speckit.codereview-summary \
  .specify/reviews/codereview-python-2024-10-29-143052.md \
  .specify/reviews/validation-2024-10-29-150230.md
# ✅ 生成：.specify/reviews/summary-2024-10-29-152015.md
```

---

## 🎨 为什么选择这些 AI 模型？

| 步骤 | 模型 | 原因 |
|------|------|------|
| 步骤 1 | Claude-4.5-thinking | 深度推理能力强，能发现复杂的逻辑问题 |
| 步骤 3 | GPT-5-Codex | 代码理解精准，擅长模式匹配和相似问题发现 |
| 步骤 4 | Claude-4.5-haiku | 文本整理能力足够，成本是 thinking 版本的 1/3 |

**这种组合既保证质量，又优化成本！**

---

## ✨ 最佳实践

### 审查范围建议

- ✅ 单次审查不超过 2000 行代码
- ✅ 按模块或功能分批审查
- ✅ 优先审查核心业务逻辑和安全相关代码
- ❌ 避免一次性审查整个大型项目

### 审查时机

最佳审查时机：
- 📌 提交 Pull Request 之前
- 📌 重要功能开发完成后
- 📌 代码重构后
- 📌 发现 bug 后排查相关代码

### 人工确认重点

在步骤 2 人工确认时，重点关注：

1. **🔒 安全问题**：SQL 注入、XSS、权限绕过、敏感信息泄露
2. **💼 业务逻辑**：是否符合需求、边界条件处理、数据一致性
3. **⚡ 性能问题**：明显的性能瓶颈、资源泄漏
4. **🔧 可维护性**：代码是否容易理解和修改

---

## ❓ 常见问题

### Q1: 必须使用"中文输出"参数吗？

**A:** 不是必须的。默认生成英文报告。

```bash
# 英文报告（默认）
speckit.codereview-python src/api

# 中文报告
speckit.codereview-python src/api 中文输出
```

### Q2: 可以跳过某些步骤吗？

**A:** 
- 步骤 1（AI 审查）：必须
- **步骤 2（人工确认）：强烈建议，这是质量保证的关键**
- 步骤 3（深度验证）：可选，但能找出更多类似问题
- 步骤 4（总结）：可选，但有助于整理和归档

对于小项目（<500 行），可以只做步骤 1 + 步骤 2。

### Q3: AI 报告有很多误报怎么办？

**A:** 这很正常！步骤 2 的人工确认就是为了过滤误报。AI 并不完美，你的专业判断才是关键。

### Q4: 报告文件需要提交到 Git 吗？

**A:** 看团队需求：
- 想要归档审查记录：提交重要的总结报告
- 只是临时审查：添加 `.specify/` 到 `.gitignore`

### Q5: 支持其他编程语言吗？

**A:** 目前内置支持：
- Python：`speckit.codereview-python`
- React/TypeScript：`speckit.codereview-react`
- Java：`speckit.codereview-java`

其他语言可以用 Python 命令作为通用工具。

### Q6: 审查过程中断了怎么办？

**A:** 完全没问题！每一步都会保存结果文件，你可以随时继续：
- 从任何步骤重新开始
- 对同一份代码进行多次审查
- 只运行某几个步骤

---

## 🚀 快速开始

现在就开始你的第一次代码审查：

```bash
# 1. 安装（只需一次）
uvx --from git+https://github.com/coderdevin/spec-kit specify init . --ai cursor --force

# 2. 选择项目类型，开始审查
speckit.codereview-python src/ 中文输出
# 或
speckit.codereview-react src/ 中文输出
# 或
speckit.codereview-java src/ 中文输出

# 3. 打开生成的报告，标记真实问题

# 4. 继续后续步骤...
```

---

## 📚 相关资源

- **[英文完整教程](./code-review-tutorial.md)** - 更详细的说明和示例
- **[快速参考卡片](./code-review-quick-reference.md)** - 打印或保存以便查阅
- **[Spec-Kit GitHub](https://github.com/coderdevin/spec-kit)** - 项目主页
- **[问题反馈](https://github.com/coderdevin/spec-kit/issues)** - 遇到问题？提 Issue

---

## 💪 总结

通过这个四步流程，你可以：

1. ✅ **快速发现问题** - AI 在几分钟内完成初步审查
2. ✅ **保证准确性** - 人工确认避免误报和不必要的修改
3. ✅ **全面覆盖** - 自动发现代码库中的相似问题
4. ✅ **优化成本** - 根据任务复杂度选择合适的 AI 模型
5. ✅ **提升质量** - 系统化的流程确保代码质量

**现在就开始吧！祝你代码审查愉快！** 🎉

---

<div align="center">
  <p>觉得有用？给我们一个 ⭐ 吧！</p>
  <a href="https://github.com/coderdevin/spec-kit">
    <img src="https://img.shields.io/github/stars/coderdevin/spec-kit?style=social" alt="GitHub stars"/>
  </a>
</div>

