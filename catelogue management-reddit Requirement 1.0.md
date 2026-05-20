# 3.1 Catalog 功能规范

---

## 1. Filter 筛选栏

| 筛选项 | 类型 | 说明 |
|---|---|---|
| Business | 下拉复选框 | 默认全选，支持多选 |
| Catalog Name | 文本框 | 模糊搜索 |
| Status | 下拉复选框 | Ready / Processing / Failed |
| Product Source | 下拉复选框 | Feed / Bulk |
| Last Updated | 日期范围 | 按最近更新时间筛选 |

---

## 2. 列表字段

| 字段 | 描述 | 平台字段 | 支持排序 | 备注 |
|---|---|---|:---:|---|
| Catalog Name | Catalog 名称 | `name` | — | — |
| Business Name | Business Name + ID | `business_id`、`Name` | — | — |
| Language | 商品语言 | `language` | — | — |
| Currency | 货币单位 | `currency` | — | — |
| Status | Catalog 健康状态（见下方状态说明） | — | — | — |
| Product Source | 商品数据来源，创建时选择的 Feed / Bulk | — | — | — |
| Products | 商品总数 | `products_count` | ✓ | — |
| Approved Products | 可投放商品数 | `approved_products_count` | ✓ | — |
| Rejected Products | 拒绝商品数 | `rejected_products_count` | ✓ | — |
| Product Set Count | Catalog 下 Set 数量 | — | ✓ | — |
| Last Import Status | 最近一次导入 / 同步状态 | — | — | — |
| Last Updated | 最近更新时间 | `modified_at` | — | — |

### Status 状态说明

| 状态值 | 含义 | 触发条件 |
|---|---|---|
| **Ready** | 数据源已处理完成 | `<Batch Create Products>` status = `CREATED` / `UPDATED`；或 `<Create Product Feed>` 创建成功 |
| **Processing** | 数据源更新中 | 数据源正在解析或同步 |
| **Failed** | 数据源解析失败 | 数据源解析异常 |

---

## 3. 操作

| 类型 | 操作 | 说明 |
|---|---|---|
| 全局操作 | Create Catalog | 创建 Catalog；创建成功后自动跳转至详情页 |
| 行操作 | View Details | 进入 Catalog 详情页，默认打开 Products Tab |
| 行操作 | Update Catalog | 更新 Catalog 基础信息 |
| 行操作 | Delete Catalog | 删除前需二次确认；若存在关联中的 Campaign，则禁用删除操作 |

---

## 4. 创建 / 更新 / 删除 Catalog

### 4.1 表单字段

| 字段 | 类型 | 必填 | 描述 | 创建可填 | 更新可改 | 平台字段 |
|---|---|:---:|---|:---:|:---:|---|
| Business Name | 下拉单选 | 是 | 所属 Business | ✓ | — | — |
| Catalog Name | 文本框 | 是 | 商品库名称 | ✓ | ✓ | `name` |
| Reddit Pixel | 下拉单选 | 否 | 绑定 Pixel，用于广告优化 | ✓ | ✓ | `pixel_id` |
| Language | 下拉单选 | 是 | 商品语言，枚举值参考 API | ✓ | — | `language` |
| Currency | 下拉单选 | 是 | 货币单位，枚举值参考 API | ✓ | — | `currency` |
| Product Source | 选择器 | 是 | 商品数据来源：Feed / Bulk | ✓ | — | — |

> **注意：** Language、Currency、Product Source 创建后不可更改（更新时置灰/只读）。

### 4.2 Product Source 选项说明

| 选项 | 说明 |
|---|---|
| **Feed** | 通过 Feed URL 定期同步商品数据 |
| **Bulk** | 通过批量文件上传导入商品数据 |

### 4.3 删除逻辑

- 点击 Delete 后弹出二次确认弹窗
- 若该 Catalog 存在**关联中的 Campaign**，Delete 操作禁用，并展示提示说明原因
