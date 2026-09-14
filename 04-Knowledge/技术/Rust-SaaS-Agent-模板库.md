---
type: knowledge
title: Rust SaaS Agent - 系统模板库
created: 2026-04-02
tags: [📚-knowledge, 🔧-tech]
source: stepclaw-workspace
---

# Rust SaaS Agent - 系统模板库

## 模板列表

### 1. inventory (库存管理) ✅ 已完成
**适用场景**: 仓库、零售、批发
**核心实体**:
- Product (商品)
- Warehouse (仓库)
- Stock (库存)
- StockIn (入库单)
- StockOut (出库单)

**功能特性**:
- 多仓库管理
- 库存预警
- 入库/出库流程
- 库存报表

---

### 2. crm (客户关系管理) 🚧 开发中
**适用场景**: 销售团队、客户跟进
**核心实体**:
- Customer (客户)
- Lead (线索)
- Opportunity (商机)
- Contract (合同)
- Task (任务)
- Note (跟进记录)

**功能特性**:
- 销售漏斗
- 客户分级
- 跟进提醒
- 业绩报表

---

### 3. hrm (人力资源) 🚧 开发中
**适用场景**: 企业人事管理
**核心实体**:
- Employee (员工)
- Department (部门)
- Attendance (考勤)
- Leave (请假)
- Payroll (薪资)
- Recruitment (招聘)

**功能特性**:
- 考勤统计
- 薪资计算
- 审批流程
- 员工档案

---

### 4. ecommerce (电商系统) 🚧 开发中
**适用场景**: 在线商城
**核心实体**:
- Product (商品)
- Category (分类)
- Order (订单)
- Cart (购物车)
- User (用户)
- Payment (支付)
- Shipping (物流)

**功能特性**:
- 商品管理
- 订单处理
- 支付集成
- 物流跟踪

---

### 5. project (项目管理) 🚧 开发中
**适用场景**: 团队协作、项目交付
**核心实体**:
- Project (项目)
- Task (任务)
- Milestone (里程碑)
- Resource (资源)
- TimeLog (工时)
- Document (文档)

**功能特性**:
- 甘特图
- 任务分配
- 进度跟踪
- 资源管理

---

### 6. accounting (财务系统) 🚧 规划中
**适用场景**: 企业财务
**核心实体**:
- Account (科目)
- Voucher (凭证)
- Invoice (发票)
- Report (报表)
- Budget (预算)

---

### 7. mes (制造执行) 🚧 规划中
**适用场景**: 生产制造
**核心实体**:
- WorkOrder (工单)
- Process (工序)
- Quality (质检)
- Equipment (设备)
- Material (物料)

---

## 模板使用

### 命令式
```bash
# 生成库存系统
gen-agent create inventory

# 生成CRM
gen-agent create crm --modules "customer,lead,opportunity"

# 生成电商
gen-agent create ecommerce --payment "alipay,wechat" --shipping "sf,yto"
```

### 交互式
```
Agent: 请选择系统类型：
       1. 库存管理 (inventory)
       2. 客户管理 (crm)
       3. 人力资源 (hrm)
       4. 电商系统 (ecommerce)
       5. 项目管理 (project)
       
User: 2

Agent: 您选择了CRM系统
       请选择需要的模块：
       [x] 客户管理
       [x] 线索管理
       [x] 商机管理
       [ ] 合同管理
       [ ] 任务管理
       
User: 全选

Agent: 正在生成CRM系统...
       ✅ 生成完成
```

### 配置文件
```yaml
# system.yaml
template: crm
name: 客户管理系统
modules:
  - customer
  - lead
  - opportunity
  - contract
  - task

features:
  - sales_funnel
  - follow_up_reminder
  - performance_report
  - data_export

custom_fields:
  customer:
    - name: 行业
      type: select
      options: [IT, 制造, 零售]
    - name: 规模
      type: select
      options: [小型, 中型, 大型]
```

## 模板扩展

### 自定义模板
```rust
// template.rs
pub struct Template {
    pub name: String,
    pub entities: Vec<Entity>,
    pub features: Vec<Feature>,
}

pub struct Entity {
    pub name: String,
    pub fields: Vec<Field>,
    pub relations: Vec<Relation>,
}
```

### 插件系统
```rust
// plugin.rs
trait Plugin {
    fn name(&self) -> &str;
    fn apply(&self, template: &mut Template);
}

// 示例插件
struct AuditPlugin;
impl Plugin for AuditPlugin {
    fn apply(&self, template: &mut Template) {
        // 为所有实体添加审计字段
        // created_at, updated_at, created_by, updated_by
    }
}
```

## 生成代码示例

### CRM系统代码结构
```
crm-system/
├── Cargo.toml
├── docker-compose.yml
└── src/
    ├── main.rs
    ├── models/
    │   ├── customer.rs      # 客户实体
    │   ├── lead.rs          # 线索实体
    │   ├── opportunity.rs   # 商机实体
    │   ├── contract.rs      # 合同实体
    │   └── task.rs          # 任务实体
    ├── handlers/
    │   ├── customer.rs      # 客户API
    │   ├── lead.rs          # 线索API
    │   ├── opportunity.rs   # 商机API
    │   └── ...
    ├── services/
    │   └── sales_funnel.rs  # 销售漏斗逻辑
    └── middleware/
        └── auth.rs          # 认证中间件
```

### 前端代码 (Leptos)
```rust
// frontend/src/pages/customer_list.rs
#[component]
fn CustomerList() -> impl IntoView {
    let customers = create_resource(
        || (),
        |_| async move {
            fetch_customers().await
        }
    );
    
    view! {
        <div class="customer-list">
            <h1>"客户列表"</h1>
            <Suspense fallback=|| view! { <p>"加载中..."</p> }>
                {move || customers.get().map(|data| {
                    view! {
                        <CustomerTable data={data}/>
                    }
                })}
            </Suspense>
        </div>
    }
}
```

## 商业化功能

### 多租户
```rust
// middleware/tenant.rs
pub async fn tenant_middleware(
    req: Request,
    next: Next,
) -> Response {
    let tenant_id = extract_tenant(&req);
    req.extensions_mut().insert(TenantContext::new(tenant_id));
    next.run(req).await
}
```

### 权限控制
```rust
// rbac.rs
pub enum Permission {
    CustomerCreate,
    CustomerRead,
    CustomerUpdate,
    CustomerDelete,
}

pub fn check_permission(user: &User, perm: Permission) -> bool {
    user.role.has_permission(perm)
}
```

### 订阅计费
```rust
// billing.rs
pub struct Subscription {
    pub tenant_id: Uuid,
    pub plan: Plan,
    pub status: SubscriptionStatus,
    pub expires_at: DateTime,
}

pub enum Plan {
    Free,
    Basic,
    Pro,
    Enterprise,
}
```

## 下一步计划

1. **完善CRM模板** - 添加完整CRUD和前端
2. **添加HRM模板** - 人事管理核心功能
3. **开发配置界面** - 可视化配置系统
4. **集成AI能力** - 智能字段推荐
5. **添加测试生成** - 自动生成单元测试

**您想先完善哪个模板？** 🤔
