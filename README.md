# Atlas 通讯录 · EE308FZ 极限编程作业

> 把重要的人，放在触手可及的地方。

Atlas 是一个面向 EE308FZ《软件工程》极限编程作业的通讯录 Web 应用。它支持**收藏联系人**、**一位联系人多条多种联系方式**，以及**基于 Excel 的导入 / 导出**，数据默认保存在浏览器本地，无需后端即可运行。

- **在线仓库**：<https://github.com/Lucas-JIN-cloud/ee308-address-book>
- **课程**：2601_FZU-MU_SE
- **作业**：极限编程（通讯录应用）
- **技术栈**：React 19 + TypeScript + Vite 8 + SheetJS
- **测试**：Vitest + Testing Library，33 个测试用例

## 功能清单

| 作业要求 | 实现情况 |
| --- | --- |
| 收藏联系人（25 分） | 联系人卡片星标、详情页收藏按钮、侧边栏「我的收藏」独立视图、收藏数量与比例、收藏优先展示 |
| 多种联系方式（10 分） | 电话 / 邮箱 / 社交账号 / 地址四类，每类可添加任意多条，每条都有自定义标签（手机、工作、微信等） |
| Excel 导出（25 分） | 每个联系人一行；姓名、公司、职位、收藏、备注有独立列；每类每种联系方式都有「标签 + 内容」列；支持导出全部 / 仅收藏；附「填写说明」工作表 |
| Excel 导入（25 分） | 拖拽或选择 `.xlsx/.xls`；自动识别中英文表头；预览、行级错误提示；智能合并 / 跳过重复 / 全部添加三种策略 |
| 响应式界面 | 桌面三栏、平板两栏、移动端列表 + 全屏详情；键盘 `Ctrl/⌘ + K` 搜索 |
| 测试 | 领域逻辑、存储、Excel 往返、UI 交互共 33 个用例 |
| 云部署（可选 +15） | 提供 Docker + Nginx、GitHub Pages、Vercel / 阿里云 / 华为云部署说明；**公开 URL 需部署后补充** |

## 快速开始

环境要求：Node.js ≥ 20（推荐 22/24）、pnpm ≥ 9（推荐 11）。

```bash
git clone https://github.com/Lucas-JIN-cloud/ee308-address-book.git
cd ee308-address-book
pnpm install
pnpm dev
```

打开终端提示的地址（默认 <http://127.0.0.1:5173>）。

> 仓库使用 `pnpm-workspace.yaml` 中的 `nodeLinker: hoisted`，以兼容 Windows 下的 TypeScript / Vite / Vitest 工具链。若使用 pnpm 11+，配置会自动生效。

### 常用命令

```bash
pnpm dev          # 本地开发
pnpm build        # 类型检查 + 生产构建（dist/）
pnpm preview      # 预览生产构建
pnpm test         # 运行全部测试
pnpm lint         # ESLint
pnpm format       # Prettier 格式化
```

## 使用说明

### 收藏联系人

- 列表卡片右上角的星标、详情页的「收藏」按钮都可以切换收藏状态。
- 侧边栏「我的收藏」只显示已收藏联系人，并显示收藏数量。
- 收藏状态会随联系人一起保存到浏览器本地，也会写进 Excel 的「收藏」列。

### 多种联系方式

新建 / 编辑联系人时，可分别为「电话、邮箱、社交账号、地址」添加多条记录。每条记录由**标签**和**内容**组成，例如：

- 电话：`手机 / 138 0013 8000`、`工作 / 010-8888 6666`
- 邮箱：`工作 / name@company.com`、`个人 / name@home.com`
- 社交：`微信 / wechat_id`、`LinkedIn / name`
- 地址：`公司 / 北京市海淀区…`、`家庭 / 杭州市西湖区…`

电话和邮箱在详情页可以直接点击拨号 / 发邮件，所有方式都支持一键复制。

### Excel 导出

点击列表右上角「导出」：

- **导出全部联系人**：所有联系人各占一行。
- **仅导出收藏**：只导出收藏联系人。
- **下载导入模板**：获得带标准表头和填写说明的空模板。

导出文件包含两个工作表：

1. `通讯录`：联系人数据，首行是字段名，已设置列宽与自动筛选。
2. `填写说明`：导出时间、字段含义、导入方式。

表头顺序：

```text
姓名 | 公司 | 职位 | 收藏 | 备注 |
电话1标签 | 电话1号码 | 电话2标签 | 电话2号码 | … |
邮箱1标签 | 邮箱1地址 | … |
社交1标签 | 社交1账号 | … |
地址1标签 | 地址1详细地址 | …
```

每种联系方式的列数会按实际最多条数自动扩展（至少 2 组）。每个联系人只占一行。收藏列写「是 / 否」。

### Excel 导入

点击「导入」（侧边栏、列表工具栏或空状态均可）：

1. 拖拽或选择 `.xlsx / .xls` 文件（最大 10 MB）。
2. 应用自动寻找表头并预览前 5 行，标出无法识别的行。
3. 选择重复联系人处理方式：
   - **智能合并**（推荐）：同名且至少一个联系方式相同，补充新联系方式并更新公司 / 职位 / 备注。
   - **跳过重复**：只导入通讯录中不存在的联系人。
   - **全部添加**：不做去重。
4. 确认导入，查看新增 / 合并 / 跳过统计。

兼容的表头包括：

- 中文：`姓名`、`公司`、`职位`、`收藏`、`备注`、`电话1标签`、`电话1号码`、`邮箱1地址`、`社交1账号`、`地址1详细地址` 等。
- 英文：`Name`、`Company`、`Job Title`、`Favorite`、`Notes`、`Phone`、`Email`、`Social`、`Address` 等。
- 简写：只有 `电话` / `手机` / `邮箱` 一列也能导入，标签会自动补为「手机 / 个人」。
- 无法识别的列会被忽略并在导入界面提示。

## 架构与模块边界

```text
src/
├── App.tsx                 # 页面状态编排：视图、选中、弹窗、导入导出
├── components/             # 展示与交互组件
│   ├── Sidebar.tsx         # 品牌、视图切换、数据操作入口
│   ├── ContactList.tsx     # 搜索后的联系人列表、星标
│   ├── ContactDetail.tsx   # 联系人档案、联系方式分组、复制
│   ├── ContactForm.tsx     # 新建 / 编辑、多值联系方式编辑器
│   ├── ImportDialog.tsx    # 文件拖拽、解析预览、重复策略
│   ├── ExportMenu.tsx      # 导出全部 / 收藏 / 模板
│   ├── Modal.tsx           # 通用对话框（Esc、滚动锁定）
│   └── ...
├── lib/
│   ├── contacts.ts         # 纯函数领域逻辑：CRUD、搜索、排序、合并
│   ├── excel.ts            # SheetJS 导入解析 / 导出工作簿
│   ├── storage.ts          # localStorage 读写、schema 校验、坏数据恢复
│   ├── format.ts           # 头像、姓名缩写、日期格式化
│   └── labels.ts           # 联系方式类型元数据
├── data/seed.ts            # 首次启动的 10 位示例联系人
├── types.ts                # Contact / ContactMethod / ContactInput
└── test/                   # 测试工厂与 setup
```

设计原则：

- **领域逻辑与 UI 解耦**：`lib/contacts.ts` 与 `lib/excel.ts` 均为纯函数，便于测试。
- **单一数据源**：`App.tsx` 持有联系人数组，任何修改都通过不可变更新产生新数组，并统一写入 localStorage。
- **可恢复存储**：读取时校验 schema，过滤坏记录与无效联系方式；版本不匹配时回退示例数据。
- **导入先预览**：解析与写库分离，用户确认后才改动通讯录。
- **可访问性**：按钮均有可读名称、星标使用 `aria-pressed`、对话框支持 Esc 与焦点管理、移动端详情可返回。

## 测试

```text
src/lib/contacts.test.ts   11 个：创建、更新、收藏、搜索、排序、统计、合并策略
src/lib/excel.test.ts       9 个：导出表头、往返、模板、中文 / 英文表头、错误行
src/lib/storage.test.ts     6 个：持久化、空通讯录、坏数据、schema 迁移
src/App.test.tsx            7 个：收藏视图、搜索、新建多电话、校验、删除
```

运行：

```bash
pnpm test
```

## 云部署（可选加分项）

前端为纯静态站点，`dist/` 可直接托管。仓库提供：

- `Dockerfile` + `nginx.conf`：任意云服务器 / 容器平台。
- `.github/workflows/deploy-pages.yml`：推送到 `main` 后自动发布 GitHub Pages。
- `docs/deployment.md`：阿里云 / 华为云 / Vercel 的逐步说明。

```bash
docker build -t atlas-contacts .
docker run -d -p 8080:80 --name atlas-contacts atlas-contacts
# 浏览器访问 http://<服务器公网IP>:8080
```

> `localhost` 链接不计入云部署加分。部署完成后请把公开 URL 填入本文档、`docs/blog-draft.md` 和 CSDN 博客。

## 项目文档

- [设计说明](docs/design.md)
- [代码规范](docs/code-standards.md)
- [Git 协作流程](docs/git-workflow.md)
- [测试计划](docs/test-plan.md)
- [部署指南](docs/deployment.md)
- [CSDN 博客草稿](docs/blog-draft.md)
- [PR 描述模板](docs/pull-request.md)

## 团队

| 角色 | 姓名 | GitHub | 主要职责 |
| --- | --- | --- | --- |
| 成员 A | 待填写 | [@Lucas-JIN-cloud](https://github.com/Lucas-JIN-cloud) | 项目脚手架、领域模型、存储、收藏、Excel、测试、文档 |
| 成员 B | 待填写 | 待填写 | UI 走查、导入导出联调、测试补充、演示视频、博客校对 |

> 这是二人小组作业。请把成员 B 的信息替换为真实队友，并确保双方都有实际提交记录；提交次数与分工需与 CSDN 博客一致。

## License

MIT
