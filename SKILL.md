---
name: brand-iso
description: "品牌化等距插画生成器：读取客户品牌色/字体/Logo，生成完全匹配品牌视觉的等距风格插画。输入品牌指南或色值，输出 SVG 等距插画（可直接用于落地页/PPT/社媒）。自动适配暗色/亮色模式。"
version: 1.0.0
author: "SCIAI 赚钱部"
price: "$29"
---

# Brand Iso — 品牌化等距插画生成器

> 你是品牌视觉设计师。你的任务是根据客户的品牌指南，生成完全匹配品牌视觉的等距（isometric）风格插画。
> 你不是在"画图"——你是在"将品牌基因注入等距插画"。

## 什么时候用这个技能
- 用户说"帮我画一个符合我们品牌的等距插画"
- 用户说"这个插画太通用了，改成我们品牌风格"
- 用户贴了品牌色值/Logo描述，要等距风格插图

---

## 第一步：提取品牌基因

从用户提供的信息中提取：

```yaml
brand:
  primary: "#4F46E5"        # 主色（必需）
  secondary: "#10B981"      # 辅色（可选，默认主色互补）
  accent: "#F59E0B"         # 强调色（可选，默认主色邻近）
  dark: "#1E1B4B"           # 暗色背景（可选，默认 #1a1a2e）
  light: "#FFFFFF"          # 亮色背景（可选）
  font: "Inter, sans-serif" # 品牌字体（可选）
  style: "现代科技"          # 品牌调性
  keywords: ["AI", "自动化", "效率"]  # 主题关键词
```

## 第二步：等距网格系统

所有等距插画基于 30° 网格：

```
等距角度：30°
网格大小：一个单位 = 40px（SVG viewBox 中）
坐标系：x 轴向右下 30°，y 轴向左下 30°，z 轴垂直向上
```

### 等距方块（基础组件）
```svg
<!-- 顶面 -->
<polygon points="100,40 140,60 100,80 60,60" fill="{primary}" />
<!-- 左面 -->
<polygon points="60,60 100,80 100,120 60,100" fill="{primary_dark}" />
<!-- 右面 -->
<polygon points="100,80 140,60 140,100 100,120" fill="{primary_darker}" />
```

### 颜色梯度规则
```
顶面：主色 100% 亮度
左面：主色 80% 亮度（暗 20%）
右面：主色 65% 亮度（暗 35%）
侧面阴影：主色 50% 亮度
```

## 第三步：等距组件库（按需组合）

### A. 服务器/数据节点
```svg
<g class="iso-server">
  <polygon points="100,40 140,60 100,80 60,60" fill="{primary}"/>
  <polygon points="60,60 100,80 100,120 60,100" fill="{primary_dark}"/>
  <polygon points="100,80 140,60 140,100 100,120" fill="{primary_darker}"/>
  <!-- LED 指示灯 -->
  <circle cx="90" cy="70" r="2" fill="{accent}"/>
  <circle cx="100" cy="75" r="2" fill="{accent}"/>
</g>
```

### B. 云/网络连接
```svg
<g class="iso-cloud">
  <path d="M 100,60 Q 120,40 140,60 Q 160,40 140,80 L 60,80 Q 40,60 60,60 Z" fill="{secondary}" opacity="0.3"/>
  <!-- 连接线（等距虚线） -->
  <line x1="80" y1="80" x2="120" y2="100" stroke="{accent}" stroke-dasharray="4,4"/>
</g>
```

### C. 人形/用户图标
```svg
<g class="iso-person">
  <circle cx="100" cy="50" r="8" fill="{primary}"/>
  <path d="M 92,58 Q 100,54 108,58 L 110,75 Q 100,79 90,75 Z" fill="{primary_dark}"/>
</g>
```

### D. 齿轮/自动化
```svg
<g class="iso-gear">
  <circle cx="100" cy="70" r="15" fill="none" stroke="{primary}" stroke-width="3"/>
  <!-- 齿 -->
  <rect x="98" y="52" width="4" height="8" fill="{primary}"/>
  <rect x="98" y="80" width="4" height="8" fill="{primary}"/>
  <rect x="82" y="68" width="8" height="4" fill="{primary}"/>
  <rect x="110" y="68" width="8" height="4" fill="{primary}"/>
</g>
```

## 第四步：组合规则

1. **主次分明**：一个视觉焦点（最大组件）+ 2-3 个辅助组件
2. **颜色分布**：主色 60% / 辅色 30% / 强调色 10%
3. **留白呼吸**：组件间距至少一个网格单位
4. **深度暗示**：前景组件比背景组件亮 10%
5. **动效预留**：关键组件加 class，方便 CSS 动画

## 第五步：输出格式

```svg
<svg viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient id="brandGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="{primary}"/>
      <stop offset="100%" stop-color="{secondary}"/>
    </linearGradient>
  </defs>
  <!-- 等距插画内容 -->
</svg>
```

## 暗色/亮色模式适配

```css
/* 默认（亮色） */
.iso-illust { background: {light}; }

/* 暗色模式 */
@media (prefers-color-scheme: dark) {
  .iso-illust { background: {dark}; }
  .iso-illust polygon { filter: brightness(0.9); }
}
```

## 使用示例

用户输入：
> "我们品牌色是 #4F46E5，帮我画一个 AI Agent 工作流的等距插画"

你的输出：
1. 品牌基因提取：primary=#4F46E5, style=科技
2. 组件选择：3 个服务器节点（Agent）+ 连接线（工作流）+ 人形（用户）
3. 完整 SVG 代码
4. 暗色/亮色适配 CSS

## 验证清单

- [ ] 所有颜色来自品牌色值（不是默认色）
- [ ] 等距角度严格 30°
- [ ] 明暗面遵循 80%/65% 规则
- [ ] 有一个视觉焦点
- [ ] SVG 无外部依赖（内联所有样式）
- [ ] 暗色模式可用

---

## 商业信息

- **产品名**：Brand Iso — Claude Code Skill Pack
- **定价**：$29 / 三件套 $69
- **与 Skill A 关系**：A 去AI味 + B 注入品牌 = 完整品牌视觉方案
- **竞品参考**：Bannerbear（$49-$299/月，API 自动生成品牌视觉）
- **版本**：v1.0.0（2026-11-25）
