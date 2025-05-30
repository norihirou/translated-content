---
title: fr
slug: Web/SVG/Reference/Attribute/fr
---

对于 {{ SVGElement("radialGradient") }} 元素，此属性用来定义径向渐变的焦点的半径。若该属性没有被定义，默认值为 0%。

## 使用说明

| 类别   | 无                                                          |
| ------ | ----------------------------------------------------------- |
| 值     | [\<length>](/zh-CN/docs/Web/SVG/Guides/Content_type#length) |
| 可变性 | 非                                                          |

## 示例

```html
<?xml version="1.0" standalone="no"?>

<svg width="120" height="120" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <radialGradient
      id="Gradient"
      cx="0.5"
      cy="0.5"
      r="0.5"
      fx="0.35"
      fy="0.35"
      fr="5%">
      <stop offset="0%" stop-color="red" />
      <stop offset="100%" stop-color="blue" />
    </radialGradient>
  </defs>

  <rect
    x="10"
    y="10"
    rx="15"
    ry="15"
    width="100"
    height="100"
    fill="url(#Gradient)"
    stroke="black"
    stroke-width="2" />

  <circle
    cx="60"
    cy="60"
    r="50"
    fill="transparent"
    stroke="white"
    stroke-width="2" />
  <circle cx="35" cy="35" r="2" fill="white" stroke="white" />
  <circle cx="60" cy="60" r="2" fill="white" stroke="white" />
  <text x="38" y="40" fill="white" font-family="sans-serif" font-size="10pt">
    (fx,fy)
  </text>
  <text x="63" y="63" fill="white" font-family="sans-serif" font-size="10pt">
    (cx,cy)
  </text>
</svg>
```

## 元素

下列元素可以使用 `fr` 属性：

- {{ SVGElement("radialGradient") }}

## 规范

{{Specifications}}
