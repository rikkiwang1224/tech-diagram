# 技术架构图 SVG 生成器 (AI Agent Skill)

专为 Cursor 和各类 Coding Agent 设计的 Prompt Skill，用于指导 AI 生成高质量、高审美的技术架构图、流程图和状态机（基于原生 SVG）。

## 💡 为什么需要这个 Skill？

目前大多数 AI Agent 在被要求画图时，默认会生成 Mermaid.js 代码。虽然 Mermaid 非常适合快速草图，但当您需要为**技术博客、官方文档或学术演示**准备配图时，它的排版和审美往往显得过于简陋和不可控。

这个 Skill 的核心理念是：**绕过 Mermaid，指导 AI 直接编写原生 SVG 代码**。通过注入一套从顶级技术文章中提取的严格设计规范（Design System），让 AI 能够稳定输出极具专业感和现代审美的图表。

## ✨ 核心特性

- **语义化莫兰迪色系**：内置青、棕、绿、陶红、紫等低饱和度专业配色，AI 会根据模块的逻辑属性自动分组上色，告别“彩虹图”。
- **完美的文本排版**：强制使用原生 SVG 的 `<text>` 和 `<tspan>` 结合 `text-anchor="middle"`，确保文本在任何 Markdown 预览器和 GitHub 页面中都能完美居中和渲染（不使用兼容性差的 `<foreignObject>`）。
- **动名词分离**：严格规范“实体（名词）”放在带边框的矩形中，而“动作/事件（动词）”作为标签悬浮在箭头上，使图表逻辑清晰、不臃肿。
- **正交连线与防遮挡**：强制使用直角折线（Orthogonal Routing），并内置了严格的 Z-Index 绘制顺序规则，确保连线永远在节点下方，且箭头文本自带背景遮罩，防止线条穿透文字。

## 📦 安装指南

1. 在您的项目中创建 `.cursor/skills/tech-diagram` 目录。
2. 将本项目中的 `SKILL.md` 文件复制到该目录中。
3. （可选）如果您使用的是其他 Agent 框架（如 Claude、GPT-4 等），只需将 `SKILL.md` 中的内容复制并粘贴到您的系统提示词（System Prompt）或自定义指令中即可。

## 🚀 使用方法

安装完成后，只需像平常一样向您的 AI Agent 下达画图指令即可：

> “画一个用户登录的流程图，包含网关、鉴权和数据库。”
> “生成我们微服务架构的组件图。”
> “画一个订单生命周期的状态机。”

AI 会自动读取 `SKILL.md` 中的规则，并输出一段优美的、可直接预览的 ````xml ... ```` SVG 代码块。

## 🎨 生成示例

以下是 AI 根据本 Skill 规则生成的“订单处理与异步事件分发”架构图示例：

```xml
<svg viewBox="0 0 700 400" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="arrow-dark" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#333333" />
    </marker>
    <marker id="arrow-blue" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#1565C0" />
    </marker>
    <marker id="arrow-teal" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#2B8C8C" />
    </marker>
    <marker id="arrow-brown" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#9C7355" />
    </marker>
    <pattern id="hatch" width="8" height="8" patternUnits="userSpaceOnUse">
      <path d="M-2,10 l12,-12 M-2,2 l4,-4 M6,10 l4,-4" stroke="#E0E0E0" stroke-width="2"/>
    </pattern>
  </defs>

  <!-- 1. 背景层 (最底层) -->
  <rect x="20" y="20" width="660" height="360" fill="#FBFBFB" fill-opacity="0.6" stroke="#BDBDBD" stroke-width="1.5" stroke-dasharray="6 4" rx="8" />
  <text x="35" y="45" font-family="system-ui, sans-serif" font-size="12" fill="#666666" font-weight="600">Order Processing Context</text>

  <!-- 2. 连线层 (在节点下方) -->
  <path d="M 120 120 L 120 160" fill="none" stroke="#333333" stroke-width="1.5" marker-end="url(#arrow-dark)" />
  <path d="M 190 200 L 240 200" fill="none" stroke="#1565C0" stroke-width="1.5" marker-end="url(#arrow-blue)" />
  <path d="M 330 235 L 330 270" fill="none" stroke="#2B8C8C" stroke-width="1.5" marker-end="url(#arrow-teal)" />
  <path d="M 410 200 L 460 200" fill="none" stroke="#2B8C8C" stroke-width="1.5" marker-end="url(#arrow-teal)" />
  <path d="M 560 230 L 560 270" fill="none" stroke="#9C7355" stroke-width="1.5" marker-end="url(#arrow-brown)" />

  <!-- 3. 动作标签层 (带遮罩) -->
  <rect x="200" y="192" width="30" height="16" fill="#FBFBFB" />
  <text x="215" y="204" font-family="system-ui, sans-serif" font-size="11" fill="#1565C0" text-anchor="middle">RPC</text>

  <rect x="420" y="192" width="30" height="16" fill="#FBFBFB" />
  <text x="435" y="204" font-family="system-ui, sans-serif" font-size="11" fill="#2B8C8C" text-anchor="middle">Pub</text>

  <!-- 4. 实体节点层 (最顶层) -->
  <rect x="50" y="70" width="140" height="50" fill="#FFFFFF" stroke="#333333" stroke-width="1" rx="4" />
  <text x="120" y="91" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="120" dy="0" font-size="13" font-weight="600" fill="#333333">External Client</tspan>
    <tspan x="120" dy="16" font-size="11" fill="#333333" opacity="0.8">REST / POST</tspan>
  </text>

  <rect x="50" y="170" width="140" height="60" fill="#E8F0FE" stroke="#1565C0" stroke-width="1.5" rx="4" />
  <text x="120" y="196" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="120" dy="0" font-size="13" font-weight="600" fill="#0D47A1">API Gateway</tspan>
    <tspan x="120" dy="16" font-size="11" fill="#0D47A1" opacity="0.8">Auth &amp; Rate Limit</tspan>
  </text>

  <rect x="250" y="165" width="160" height="70" fill="#E8F4F4" stroke="#2B8C8C" stroke-width="1.5" rx="4" />
  <text x="330" y="196" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="330" dy="0" font-size="13" font-weight="600" fill="#1A5C5C">Order Engine</tspan>
    <tspan x="330" dy="16" font-size="11" fill="#1A5C5C" opacity="0.8">State Machine Tx</tspan>
  </text>

  <rect x="250" y="280" width="160" height="40" fill="url(#hatch)" stroke="#333333" stroke-width="1" rx="4" />
  <text x="330" y="304" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="330" dy="0" font-size="11" font-weight="600" fill="#333333" letter-spacing="1">SYNC BARRIER (DB)</tspan>
  </text>

  <rect x="470" y="170" width="180" height="60" fill="#F9F3EE" stroke="#9C7355" stroke-width="1.5" rx="30" />
  <text x="560" y="196" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="560" dy="0" font-size="13" font-weight="600" fill="#5C402C">Message Bus</tspan>
    <tspan x="560" dy="16" font-size="11" fill="#5C402C" opacity="0.8">topic: order.created</tspan>
  </text>

  <rect x="470" y="280" width="180" height="60" fill="#F4F0FA" stroke="#7E57C2" stroke-width="1.5" rx="4" />
  <text x="560" y="306" font-family="system-ui, sans-serif" text-anchor="middle">
    <tspan x="560" dy="0" font-size="13" font-weight="600" fill="#4527A0">Inventory Worker</tspan>
    <tspan x="560" dy="16" font-size="11" fill="#4527A0" opacity="0.8">Async deduction</tspan>
  </text>
</svg>
```
