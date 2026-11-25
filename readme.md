%%{init: {'theme': 'base', 'flowchart': {'nodeSpacing': 28, 'rankSpacing': 45, 'curve': 'basis'}, 'themeVariables': {'fontFamily': 'Segoe UI, Arial, sans-serif', 'fontSize': '13px', 'primaryColor': '#FFFFFF', 'primaryBorderColor': '#93C5FD', 'lineColor': '#0B65B8', 'background': '#FFFFFF'}}}%%
flowchart LR
    subgraph CANVAS[" "]
        direction LR
        subgraph PLANT["PLANT SIDE"]
            direction TB
            DB1[("MS SQL Server\nRM 1-30")]
            DB2[("MS SQL Server\nRM 31-60")]
            NET(("Plant Core\nLAN Switch"))

            DB1 --> NET
            DB2 --> NET
        end

        subgraph WAN["  DEDICATED OPTICAL FIBER POOL  "]
            direction TB
            PTCL["PTCL Dedicated Fiber\n150 Mbps - Primary"]
            WATEEN["Wateen Dedicated Fiber\n50 Mbps - Failover"]
        end

        subgraph HO["HEAD OFFICE (HO SIDE)"]
            direction TB
            APP["Portal + Apache Superset Server\nWeb Application & Real-Time BI"]
            LLM["LLM Server\nCentralized AI Inference"]

            APP <--> LLM
        end

        NET <==>|"Primary Fiber"| PTCL
        NET <-.->|"Failover Fiber"| WATEEN
        PTCL <==>|"150 Mbps P2P"| APP
        WATEEN <-.->|"50 Mbps P2P"| APP
    end

    classDef db fill:#0b65b8,stroke:#084c8d,stroke-width:2px,color:#ffffff
    classDef net fill:#ffffff,stroke:#0b65b8,stroke-width:2px,color:#0b65b8
    classDef server fill:#e6f0fa,stroke:#0b65b8,stroke-width:2px,color:#083358
    classDef llm fill:#dbeafe,stroke:#1d4ed8,stroke-width:2px,color:#1e3a8a
    classDef wanPrimary fill:#ecfdf5,stroke:#059669,stroke-width:2px,color:#065f46
    classDef wanBackup fill:#fffbeb,stroke:#d97706,stroke-width:1.5px,stroke-dasharray: 4 4,color:#92400e

    class DB1,DB2 db
    class NET net
    class APP server
    class LLM llm
    class PTCL wanPrimary
    class WATEEN wanBackup

    style CANVAS fill:#ffffff,stroke:#ffffff,stroke-width:0px
    style PLANT fill:#f8fbff,stroke:#93c5fd,stroke-width:1.5px,color:#083358
    style WAN fill:#fffdf9,stroke:#cbd5e1,stroke-width:1.5px,stroke-dasharray: 4 4,color:#1e293b
    style HO fill:#eff6ff,stroke:#0b65b8,stroke-width:2px,color:#0b65b8
