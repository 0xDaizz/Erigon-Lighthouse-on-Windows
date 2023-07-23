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

- install grafana on this website
[grafana installation link](https://grafana.com/grafana/download?pg=get&plcmt=selfmanaged-box1-cta1&platform=windows)

- Open this Json file in grafana dashboard
[Erigon Json Import](https://github.com/ledgerwatch/erigon/blob/devel/cmd/prometheus/dashboards/erigon.json)

---
## 2. Lighthouse

- Before doing this, need to know the dependencies
```
choco install make llvm protoc
choco install cmake --installargs 'ADD_CMAKE_TO_PATH=System'
(Rust install)
```

### 2-1.
- Build
  ```
  # start fom ~/
  cd ~
  
  git clone https://github.com/sigp/lighthouse.git
  cd lighthouse

  git checkout stable

  # Build with this!
  $env:PROFILE = "maxperf"; make


  # Run
  ```lighthouse bn --network mainnet --execution-endpoint http://localhost:8551 --execution-jwt $HOME\data\jwt.hex --checkpoint-sync-url https://mainnet.checkpoint.sigp.io --disable-deposit-contract-sync --datadir $HOME\data\lighthouse_data```


### 2-2
  
