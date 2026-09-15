# 🌍 TerraSense

## Multi-Hazard Environmental Early Warning Network

> **Sense Locally • Analyze Intelligently • Alert Early**

TerraSense is a distributed environmental monitoring and early-warning platform designed to identify changing environmental conditions and provide localized warnings before a situation becomes critical.

The platform is designed around a common architecture consisting of **sensor nodes, ESP32 controllers, 433 MHz LoRa communication, local processing, intelligent risk assessment, and multi-level alerts**.

TerraSense is designed as a **multi-hazard system** covering:

- 🌍 Landslides
- 🌊 Floods
- 🔥 Forest Fires
- 🏭 Environmental Pollution

### Current Development Focus

The **current working prototype focuses on landslide monitoring and early warning**.

The other three hazard nodes — flood, forest fire, and pollution — represent the planned expansion of the same platform.

The objective is not to build four completely different systems, but to create **one common monitoring architecture that can support different hazards by changing the sensing layer and risk parameters.**

---

# 🎯 Problem

Environmental hazards are often preceded by measurable changes in surrounding conditions.

For example:

- Increasing soil moisture and rainfall can contribute to landslide risk.
- Rapidly increasing water levels can indicate flood risk.
- Increasing temperature, decreasing humidity and smoke can indicate forest-fire risk.
- Increasing particulate matter and gas concentrations can indicate pollution events.

However, monitoring these parameters individually does not always provide enough information to understand the overall risk.

Remote and vulnerable locations can also face unreliable internet connectivity and limited access to continuous monitoring.

TerraSense addresses this by combining **multiple sensor parameters, local processing, wireless communication and risk assessment** into a single distributed platform.

---

# 💡 Proposed Solution

TerraSense places specialized sensor nodes near areas that need continuous environmental monitoring.

Each node:

1. Collects environmental data.
2. Performs initial processing and filtering.
3. Extracts useful measurements and trends.
4. Sends summarized information using 433 MHz LoRa.
5. The gateway receives and processes the information.
6. Multiple parameters are combined to estimate risk.
7. The system generates a risk level.
8. Local and remote alerts are triggered when required.

# 🏗️ Core Architecture

TerraSense is designed as a **distributed, modular, and offline-capable environmental early-warning platform**.

The system consists of specialized sensor nodes for different hazards, local edge processing, 433 MHz LoRa communication, an ESP32 gateway, intelligent risk assessment, and a multi-level alert system.

> **Current working focus:** Landslide Early Warning  
> **Future expansion:** Flood • Forest Fire • Environmental Pollution

---

## 🔄 End-to-End Architecture

```mermaid
flowchart TB

    ENV["🌍 ENVIRONMENTAL CONDITIONS"]

    subgraph NODES["📡 01 — DISTRIBUTED SENSOR NODES"]

        L["🌍 LANDSLIDE NODE<br/><br/>
        ESP32<br/>
        MPU6050<br/>
        Soil Moisture<br/>
        Rain Sensor<br/>
        HX711 + Load Cell"]

        F["🌊 FLOOD NODE<br/><br/>
        ESP32<br/>
        Water Level<br/>
        Rain<br/>
        Temperature<br/>
        Humidity"]

        FIRE["🔥 FOREST FIRE NODE<br/><br/>
        ESP32<br/>
        Temperature<br/>
        Humidity<br/>
        Smoke / PM<br/>
        Gas / VOC"]

        POL["🏭 POLLUTION NODE<br/><br/>
        ESP32<br/>
        PM2.5 / PM10<br/>
        CO / VOC<br/>
        NO₂ / SO₂<br/>
        Temperature / Humidity"]
    end

    subgraph EDGE["⚙️ 02 — EDGE PROCESSING"]

        ACQ["Sensor Data Acquisition"]
        VALID["Data Validation"]
        FILTER["Noise Filtering"]
        FEATURE["Feature Extraction"]
        TREND["Trend & Rate-of-Change Analysis"]

        ACQ --> VALID --> FILTER --> FEATURE --> TREND
    end

    subgraph COMM["📡 03 — COMMUNICATION"]

        LORA["SX1278 / RA-02<br/>
        433 MHz LoRa<br/><br/>
        Low Power • Long Range • Offline"]
    end

    subgraph GW["🖥️ 04 — ESP32 GATEWAY"]

        RX["LoRa Packet Reception"]
        PARSE["Packet Parsing"]
        AGG["Multi-Node Aggregation"]
        PROC["Hazard-Specific Processing"]

        RX --> PARSE --> AGG --> PROC
    end

    subgraph INTEL["🧠 05 — INTELLIGENT RISK ASSESSMENT"]

        FUSION["Multi-Sensor Risk Fusion"]
        SCORE["Risk Score"]
        ML["ML-Based Risk Prediction<br/>(Development Stage)"]

        FUSION --> SCORE
        ML --> SCORE
    end

    subgraph DECISION["🚦 06 — DECISION LAYER"]

        SAFE["🟢 SAFE<br/>Normal Conditions"]
        WARN["🟡 WARNING<br/>Increasing / Potential Risk"]
        DANGER["🔴 DANGER<br/>High Risk"]
    end

    subgraph RESPONSE["🚨 07 — RESPONSE LAYER"]

        LOCAL["Local Alerts<br/><br/>
        LED • Buzzer • Voice"]

        DASH["Local Dashboard<br/><br/>
        Sensor Data • Trends<br/>
        Risk Status • Node Health"]

        REMOTE["Optional Remote Services<br/><br/>
        Notifications • Authority Alerts<br/>
        Historical Analytics"]
    end

    ENV --> L
    ENV --> F
    ENV --> FIRE
    ENV --> POL

    L --> ACQ
    F --> ACQ
    FIRE --> ACQ
    POL --> ACQ

    TREND --> LORA
    LORA --> RX

    PROC --> FUSION

    SCORE --> SAFE
    SCORE --> WARN
    SCORE --> DANGER

    SAFE --> LOCAL
    WARN --> LOCAL
    DANGER --> LOCAL

    SCORE --> DASH
    DASH -. Optional Connectivity .-> REMOTE
