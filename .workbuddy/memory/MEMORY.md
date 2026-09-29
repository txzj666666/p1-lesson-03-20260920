# p1-lesson-03 项目长期备忘

## 字体子集是手工裁剪的，改文字必须重新生成
- `assets/noto-serif-sc-400.woff2` / `-600.woff2` 是**按页面实际用字裁剪的子集字体**（各约 75KB、452 字）。
- 页面一旦新增/修改文字，新字符很可能不在子集里 → 浏览器回退到系统字体，出现「同一个词两种字体」的现象（2026-09-29 的「在读」就是「读」缺字导致）。
- **处理流程**（fontTools 已装在 `C:/Users/Admin/.workbuddy/binaries/python/envs/default`）：
  1. 汇总两个页面全部去重字符 + ASCII + 常用标点；
  2. `https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;600&text=<urlencode>` 拿到按字符裁剪的**可变字体**（400/600 返回同一份，wght 200–900）；
  3. 用 `fontTools.varLib.instancer` 固化成 wght=400 / 600 两个**静态** woff2（直接用可变字体有回退到 ExtraLight 的风险）；
  4. 校验：usWeightClass 正确、无 fvar、页面字符 0 缺失。
- 校验脚本思路见 2026-09-29 日志；临时文件用完已删。

## 其他约定
- 新增栏目复用 `section → container → section-heading/section-content` 结构；样式只写在 `resume-enhance.css`，不动 `styles.css` / `lesson-03-task.css`。
- 图片放 `assets/` 并用英文文件名。
- `index.html` / `hometown.html` 改完用 html.parser 跑一遍标签闭合校验。
- Git 提交别漏 `hometown.html` 和 `assets/` 里的新图、新字体。
