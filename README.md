# Prometheus Course Outline (English Translation)

### Chapter 1: Introductory and Basic Concepts

This chapter introduces the Prometheus monitoring tool, its services, and the fundamental concepts of monitoring.

| Topic | Key Details | Sources |
| :--- | :--- | :--- |
| **Course Roadmap** | The course includes: Introduction, Architecture, Installation (Bare Metal & Docker), Adding Grafana, Node Exporter, PromQL, and Instruments. Explanations rely primarily on the official **Prometheus documentation**. | |
| **What is Prometheus?** | A **monitoring tool** used to observe system resource consumption. It was first developed by **SoundCloud** and is written in **GoLang**. | |
| **Mode of Operation** | It operates using a **Pull Base** mechanism. | |
| **History and Importance** | Prometheus is the **second project** (after Kubernetes) to join the **CNCF** standard. It offers high flexibility, an implemented alerting system, and boasts an **active community**. | |
| **Time Series Data (TSDB)** | Data that have a **time dependency** and whose value changes over time. Tools in this domain include **Prometheus**, **InfluxDB**, and **OpenTSDB**. | |
| **Importance of Time Series** | Helps in identifying **patterns and trends**. Enables **Prediction** of system behavior and **Anomaly Detection**. Also crucial for **process optimization**. | |
| **3 Pillars of Observability** | **Logs** (program function descriptions), **Metrics** (numerical information) and **Tracing** (tracking program activities). | |
| **Common Logging Tools** | Loki, and EFK (**Elasticsearch, Fluentd, Kibana**). | |
| **Common Tracing Tools** | Tempo, and Jaeger (pronounced Yager). | |
| **Metrics** | Numerical data (a number) that refer to a measurable aspect of a system, such as CPU consumption. | |
| **Four Golden Signals of Monitoring** | **Latency** (response delay), **Traffic** (request rate), **Errors** (error count), and **Saturation** (resource consumption). | |
| **Metric Storage Structure** | A metric must have a **Name**, **Labels** (for filtering/grouping), a **Numerical Value** (usually 64-bit float in Prometheus), and a **Timestamp**. | |
| **Prometheus Data Types** | **Counter** (only increasing, e.g., request counts or error counts). **Gauge** (can increase or decrease, e.g., CPU temperature or memory consumption). **Histogram** (measures data distribution in specific intervals/buckets). **Summary** (for precise statistical calculations, particularly quantiles/percentiles). | |
| **Labels** | Assist in **filtering** and achieving **better data analysis**. Default Prometheus labels are **Job** and **Instance**. | |

### Chapter 2: Prometheus Internal Architecture

This chapter focuses on how Prometheus collects data and the components that make up the system.

| Topic | Key Details | Sources |
| :--- | :--- | :--- |
| **Metric Collection Models** | **Pull Model** (Prometheus requests data from the client/target) and **Push Model** (Client sends data to the server). | |
| **Advantages of Pull Model** | Simplicity of implementation, easier control, reduced load on resources. This is the **Prometheus default mechanism**. | |
| **Disadvantages of Pull Model** | Can suffer from slowness/latency. The target must be visible and accessible on the network. | |
| **Pushgateway** | A tool for implementing the push mechanism. Suitable for **short-lived systems** or when ** direct network access** is unavailable. It holds data short-term, and new data overrides the old. | |
| **Prometheus Server Architecture** | The **core** includes the **HTTP Server** (for viewing data and API access) and the **TSDB** (for storing time-series data). | |
| **Retrieval Component** | Its function is to **request (scrape)** data from peripheral systems. | |
| **Exporters** | Tools installed on the target system (e.g., Node Exporter for Linux) that collect various metrics (e.g., system resources, databases) and make them pullable by Prometheus. | |
| **Client Libraries** | Used to send metrics directly via API from within the application code, bypassing the need for a separate exporter. | |
| **Service Discovery** | A mechanism for automatically identifying targets that are **dynamically added or removed** (e.g., Docker images or Kubernetes services). Tools like Consul can be used for this. | |
| **Alert Manager** | Used for defining **rules** and **sending Alerts** when specific thresholds are met (e.g., CPU consumption reaches 99%). | |
| **Visualization** | The Grafana tool is recommended for improved display and visualization of data. | |
| **Recording Rule** | Defined to **pre-compute** complex or frequently needed PromQL expressions to reduce query time and computational load. | |

### Chapter 3: Prometheus Bare Metal Installation and Initial Setup

This chapter covers the practical steps for installing Prometheus and the Node Exporter on a local Linux system.

| Topic | Key Details | Sources |
| :--- | :--- | :--- |
| **Operational Scenario** | Installing Prometheus Server and three Node Exporters (on ports 8088, 8081, 8082). Grouping these nodes (Production and Canary). Running queries and defining a simple rule. | |
| **Prometheus Installation** | Downloading the Prometheus binary (e.g., Linux 64bit version), extracting it (Untar), and executing the binary file. | |
| **Configuration File (`prometheus.yml`)** | Contains **Global** variables and **Scrape Configs**. | |
| **Setting `scrape_interval`** | Defines the data collection interval. Default is 15 seconds globally, but can be **overridden** for each specific `job`. In the scenario, it was set to 5 seconds for the Node Exporter job. | |
| **Defining Job and Targets** | Every monitoring process requires a defined `job_name` and a list of `targets` (servers and ports). | |
| **Node Exporter Installation** | Downloading and running the Node Exporter on various ports (e.g., 8088, 8081, 8082 in the scenario). | |
| **Target Configuration** | Targets are added under `scrape_configs` in `prometheus.yml`. Custom labels like `group: production` and `group: canary` were assigned. | |
| **Checking Status** | The status of the targets should be checked in the Prometheus user interface (the `/targets` page) to ensure they are **UP**. | |
| **System Stability (Systemd Service)** | To ensure Prometheus or Node Exporters do not stop after a system restart, a **Linux Systemd Service** must be defined for them. | |
| **PromQL and Querying** | PromQL is used for analyzing time series data. Examples include using `rate()` to calculate the average consumption of system resources (like CPU). | |
| **Implementing a Simple Rule** | A Recording Rule was defined to **pre-compute** the average CPU consumption across nodes, reducing query time. | |
