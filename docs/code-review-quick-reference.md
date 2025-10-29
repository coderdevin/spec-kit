# 代码审查快速参考

> 快速参考卡片 - 打印或保存此页面以便随时查阅

## 🚀 一键安装

```bash
uvx --from git+https://github.com/coderdevin/spec-kit specify init . --ai cursor --force
```

---

## 📋 四步审查流程

### 1️⃣ AI 初步审查
**模型**: Claude-4.5-thinking  
**用途**: 发现潜在问题

```bash
# Python
speckit.codereview-python <目录/文件> 中文输出

# React
speckit.codereview-react <目录/文件> 中文输出

# Java
speckit.codereview-java <目录/文件> 中文输出
```

**输出**: `.specify/reviews/codereview-*.md`

---

### 2️⃣ 人工确认
**操作**: 打开报告，标记真实问题

1. 打开 `.specify/reviews/` 中的报告
2. 阅读每个问题
3. 在任务列表中打勾 `[x]` 标记真实问题
4. 保存文件

**判断标准**:
- ✅ 打勾: 安全漏洞、逻辑错误、性能问题
- ❌ 忽略: 风格偏好、误报、不适用建议

---

### 3️⃣ 深度验证
**模型**: GPT-5-Codex  
**用途**: 验证问题，找出类似问题

```bash
speckit.codereview-check <第二步的报告文件>
```

**输出**: `.specify/reviews/validation-*.md`

---

### 4️⃣ 生成总结
**模型**: Claude-4.5-haiku  
**用途**: 整合报告，生成最终总结

```bash
speckit.codereview-summary <审查报告> <验证报告>
```

**输出**: `.specify/reviews/summary-*.md`

---

## 💡 命令示例

### 完整流程示例

```bash
# 步骤 1: 初步审查
speckit.codereview-python src/api 中文输出
# 生成: .specify/reviews/codereview-python-2024-10-29-143052.md

# 步骤 2: 人工确认
# 打开上述文件，标记问题，保存

# 步骤 3: 深度验证
speckit.codereview-check .specify/reviews/codereview-python-2024-10-29-143052.md
# 生成: .specify/reviews/validation-2024-10-29-150230.md

# 步骤 4: 生成总结
speckit.codereview-summary \
  .specify/reviews/codereview-python-2024-10-29-143052.md \
  .specify/reviews/validation-2024-10-29-150230.md
# 生成: .specify/reviews/summary-2024-10-29-152015.md
```

---

## 🎯 最佳实践

| 场景 | 建议 |
|------|------|
| **审查范围** | 单次不超过 2000 行代码 |
| **审查时机** | PR 前、重构后、重要功能完成后 |
| **关注重点** | 安全、业务逻辑、性能、可维护性 |
| **报告语言** | 加"中文输出"得到中文报告 |
| **文件管理** | 定期清理 `.specify/reviews/` 旧报告 |

---

## 🔧 支持的语言

| 语言 | 命令 |
|------|------|
| **Python** | `speckit.codereview-python` |
| **React** | `speckit.codereview-react` |
| **Java** | `speckit.codereview-java` |

---

## ❓ 快速问答

**Q: 可以跳过某些步骤吗？**  
A: 可以，但不推荐。人工确认（步骤2）是关键。

**Q: 误报怎么办？**  
A: 在步骤2不打勾即可，你的判断是关键。

**Q: 报告太长？**  
A: 缩小审查范围，按模块分批审查。

**Q: 英文还是中文？**  
A: 默认英文，加"中文输出"参数得到中文报告。

---

## 📚 更多资源

- [完整教程](./code-review-tutorial.md)
- [Spec-Kit GitHub](https://github.com/coderdevin/spec-kit)
- [文档首页](./README.md)

---

**提示**: 将此页面添加到书签，方便随时查阅！

