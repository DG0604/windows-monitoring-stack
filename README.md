# Windows Infrastructure Monitoring Stack

A production-ready, secure monitoring solution utilizing Docker Compose to deploy Prometheus and Grafana, designed to ingest and visualize core enterprise metrics from a Windows Server architecture via `windows_exporter`.

## 🛠️ Tech Stack & Architecture
* **Data Source**: Windows Exporter (Metrics collector tracking CPU, memory, logical disk, and system threads)
* **Time-Series Database**: Prometheus (Metrics scraping and storage)
* **Visualization Engine**: Grafana (Real-time analytics and custom operational dashboards)
* **Deployment**: Docker Compose (Containerization and orchestration)

## 🔐 Security & Secrets Management
This project implements production-grade secret isolation using **Environment Variable Expansion**. 
* Sensitive configurations (like the Grafana admin password) are decoupled from the infrastructure code and managed locally via an `.env` file.
* Git tracking for `.env` is blocked globally via `.gitignore` to prevent accidental credential leakage to public repositories.
* A template file (`.env.example`) is provided to guide external deployment setups safely.

## 🚀 Troubleshooting & Engineering Wins
Building this monitoring stack involved overcoming complex, real-world infrastructure and data aggregation hurdles:

* **Windows Service & Registry Customization (Error Code 1)**: Troubleshot background service launch failures by executing the collector binary directly in the Command Prompt to capture low-level event logs. Bypassed rigid configuration constraints by reconfiguring the Windows Registry `ImagePath` to explicitly activate core and time-offset collectors (`time`, `system`).
* **PromQL Label Retention & Cardinality**: Resolved dynamic Grafana dashboard tracking issues where standard aggregation functions stripped target labels. Rewrote advanced PromQL queries leveraging explicit label retention strategies (`count by (instance)`) to maintain cross-server dashboard variable compatibility.
* **Collector Fine-Tuning**: Shifted high-cardinality performance monitoring away from resource-heavy individual process counters (`windows_process_thread_count`) over to lightweight system-level kernel threads (`windows_system_threads`) to preserve server resources.

## 🏃 How to Run Locally

1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
   cd your-repo-name
2. Set up your local environment file:

   Bash
   cp .env.example .env
   (Open .env and configure your secure dashboard password).

3. Launch the stack:

   Bash
   docker compose up -d

4. Access Grafana at http://localhost:3000.
