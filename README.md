# Erigon-Lighthouse-on-Windows
to memorize my instruction to build a Erigon + Lighthouse archive node on Windows computer

## 0. Spec
- Ryzen 9 7900X (Eco mode turned on to reduce the heat from CPU)
- 32GB RAM
- WD SN850X 4TB NVMe SSD

---
## 1. Prometheus + Grafana

- before doing this: Install Chocolatey (Powershell Admin mode required)
  ```
  Set-ExecutionPolicy RemoteSigned

  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
  ```

### 1-1. Prometheus
- install
  `choco install prometheus`

- open prometheus.yml
  `notepad C:\ProgramData\chocolatey\lib\prometheus\tools\prometheus-2.2.1.windows-amd64\prometheus.yml` (The directory path may vary due to your situation)

- edit
  ```
  global:
  scrape_interval: 10s
  scrape_timeout: 3s
  evaluation_interval: 5s

  scrape_configs:
  - job_name: erigon # example, how to connect prometheus to Erigon
    metrics_path: /debug/metrics/prometheus
    scheme: http
    static_configs:
      - targets:
          - localhost:6060
  ```
- Run
  turn on "prometheus-service" on the task manager -> services

### 1-2. Grafana
Open this Json file in grafana dashboard
[Erigon Json Importer](https://github.com/ledgerwatch/erigon/blob/devel/cmd/prometheus/dashboards/erigon.json)

