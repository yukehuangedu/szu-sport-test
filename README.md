# 深圳大学 · 体测免测与医保使用指南

纯静态网页，无框架、无构建步骤。改文字就是改 HTML，换图就是换 `assets/img/` 里的文件。

## 目录

```
.
├── index.html            首页
├── sport.html            体测免测 / 缓测申请
├── medical.html          粤海校区学生医保使用
├── assets/
│   ├── css/style.css     全站样式（三页共用）
│   └── img/              脱敏后的图片，全部在这里
├── README.md
└── .gitignore            排除原始素材与本地配置
```

原始截图（含姓名、学号、病史）和处理脚本不在仓库里，已移到项目外的 `../_szu-sport-raw/` 备份。

## 本地预览

```bash
python -m http.server 8000
```

浏览器打开 http://127.0.0.1:8000 。直接双击 HTML 也能看，但图片和样式用相对路径，起服务更稳。

## 改文字

打开对应 HTML，找到那段中文直接改。几个常用位置：

| 想改什么 | 去哪里找 |
|---|---|
| 时间节点、受理窗口 | `<div class="deadline">` |
| 目录锚点 | `<nav class="toc">` |
| 正文分节 | `<section id="xxx">` |

## 换图

图片统一放在 `assets/img/`，文件名见下表。换成同名文件即可，不用改 HTML。

| 文件 | 内容 | 用在哪 |
|---|---|---|
| `sport-01-approval-rules.jpg` | 缓免测审批四类口径 | 体测页「审核口径」 |
| `sport-03-attachments.jpg` | 提交的附件列表（11 份材料） | 体测页「办理流程」 |
| `sport-04-approval-flow.jpg` | 两级审批记录 | 体测页「办理流程」 |
| `sport-05-apply-form.jpg` | 申请表单 | 体测页「办理流程」 |
| `sport-06-scholarship-score.jpg` | 奖学金体测标准公文 | 体测页「和奖学金、毕业的关系」 |
| `sport-07-scholarship-cond.jpg` | 体测/义工时/思政学时公文 | 体测页「和奖学金、毕业的关系」 |
| `sport-08-teacher-notice.jpg` | 某学院主管体育老师的通知 | 体测页「和奖学金、毕业的关系」 |
| `medical-02-entry.jpg` | 深圳医保公众号入口 | 医保页「怎么改社康」 |
| `medical-03-home.jpg` | 办事首页 | 医保页「怎么改社康」 |
| `medical-04-xuandian.jpg` | 门诊选点菜单 | 医保页「怎么改社康」 |
| `medical-05-set-shequ.jpg` | 选粤桂社康 | 医保页「怎么改社康」 |
| `medical-06-bills-sep.jpg` | 结算记录（9 月） | 医保页「真实账单」 |
| `medical-07-bills-jul.jpg` | 结算记录（7 月住院） | 医保页「真实账单」 |
| `medical-08-bills-jun-late.jpg` | 南山门诊大额结算 | 医保页「真实账单」 |
| `medical-09-bills-jun-early.jpg` | 结算记录（6 月校医院） | 医保页「真实账单」 |
| `medical-07-clinic-map.jpg` | 粤桂社康地图定位 | 医保页「校医院怎么用」 |

新增图片的标准写法：

```html
<figure class="shot">
  <a href="assets/img/你的图.jpg" target="_blank" rel="noopener">
    <img src="assets/img/你的图.jpg" alt="图片内容简述" loading="lazy">
  </a>
  <figcaption><b>图 N</b>｜一句话说明这张图在讲什么</figcaption>
</figure>
```

- `class="shot"` 用于手机截图，会限制最大宽度 400px 居中；横版图去掉这个 class 即可。
- 图注统一格式：`图 N｜说明`。编号在页内连续。

## 图片预处理

换图时把新截图放进 `assets/img/` 覆盖同名文件即可，页面不用改。

如果需要重新处理原始截图（裁状态栏、打码姓名、压缩），脚本在 `../_szu-sport-raw/tools/prep_images.py`，原始图也在那个目录。脚本里的 `masks` 是马赛克矩形（原图坐标），换截图时需要调坐标。

两点经验：

- 手机截图导出 760px 宽、质量 82 就够；横向公文和地图要 900–1600px、质量 90 以上，否则文字发糊。
- 浅灰色斜排水印用马赛克会伤正文，改用 `亮度 1.10 + 对比度 1.30` 可以把水印推向纯白而不损黑字。

## 发布到 GitHub Pages

仓库已连好远程 `yukehuangedu/szu-sport-test`。

1. 推到 `main`：
   ```bash
   git add -A
   git commit -m "add guide site"
   git push origin main
   ```
2. 仓库 Settings → Pages → Source 选 `Deploy from a branch`，分支选 `main`，目录选 `/ (root)`。
3. 等 1–2 分钟，访问 `https://yukehuangedu.github.io/szu-sport-test/`。

## 维护提醒

- 体测政策每年秋季更新一次，受理窗口、材料要求、奖学金分数标准都可能变。开学后对着最新公文通核一遍。
- 医保报销比例随政策调整，页面里写的是「约 50%」「55% 左右」，不要改成精确数字。
- 粤桂社康已从桂庙新村 40 号搬走，各医疗平台仍显示旧址。地址以高德地图截图为准，不要按旧平台信息改回去。
