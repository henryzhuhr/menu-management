# 菜单中心架构图

## 整体架构

```mermaid
graph TB
    subgraph ADMIN["管理平面"]
        PM[平台管理员<br/>创建版本 / 推进通道 / 租户管理]
    end

    subgraph APPS["应用层（业务方）"]
        A1[支付业务<br/>独立版本线]
        A2[电商业务<br/>独立版本线]
    end

    subgraph K8S["K8s 集群"]
        subgraph NS_EAP["EAP Namespace"]
            SVC_EAP["菜单服务（EAP）<br/>app → version 映射"]
        end
        subgraph NS_STD["STD Namespace"]
            SVC_STD["菜单服务（STD）<br/>app → version 映射"]
        end
        subgraph NS_KA["KA Namespace"]
            SVC_KA["菜单服务（KA）<br/>app → version 映射"]
        end
    end

    subgraph DB["数据层"]
        MYSQL[("共享数据库<br/>menu_version 快照<br/>(app, version)")]
    end

    subgraph TENANTS["租户层"]
        T1["Tenant-EAP_01<br/>绑定 EAP 通道"]
        T2["Tenant-STD_01<br/>绑定 STD 通道"]
        T3["Tenant-KA_01<br/>绑定 KA 通道"]
    end

    PM --> A1
    PM --> A2
    A1 --> NS_EAP
    A1 --> NS_STD
    A1 --> NS_KA
    A2 --> NS_EAP
    A2 --> NS_STD
    A2 --> NS_KA
    SVC_EAP --> MYSQL
    SVC_STD --> MYSQL
    SVC_KA --> MYSQL
    T1 --> SVC_EAP
    T2 --> SVC_STD
    T3 --> SVC_KA
```

## 多应用 + 单服务实例：按应用映射

```mermaid
graph LR
    subgraph CONTEXT["STD 服务实例的请求"]
        T_STD["Tenant-STD_01<br/>订阅：支付 + 电商"]
    end

    subgraph SVC["STD Namespace 菜单服务"]
        SVC["{支付: v5, 电商: v2}"]
    end

    subgraph DB2["数据库"]
        DB_A["支付 v5"]
        DB_B["支付 v4"]
        DB_C["电商 v2"]
    end

    T_STD -->|app=支付| SVC
    T_STD -->|app=电商| SVC
    SVC -->|按 app + version 查询| DB2

    style SVC fill:#2f6fed,color:#fff
```

STD 服务实例通过应用-版本映射同时服务支付和电商。支付推进到 v5 不会改变电商的 v2；如果支付还需要独立升级服务代码，再将支付绑定到独立运行组。
