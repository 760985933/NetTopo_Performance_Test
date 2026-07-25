# NetTopo Performance Test Tool

[**English**](README_EN.md) | [中文](README.md) | [日本語](README_JA.md)

NetTopo is a network topology performance testing management and report generation tool, supporting CLI, Web, and desktop GUI modes.

## Features

- **FIO Configuration Generator** — Visual editor for FIO test parameters, auto-generates `.fio` scripts
- **Remote Task Execution** — Batch deploy, run, and monitor FIO tests via SSH
- **Automatic Data Retrieval** — One-click result data download from all nodes after testing
- **Smart Report Generation** — Auto-generates Excel summary + interactive HTML chart reports
- **Task Orchestration** — Drag-and-drop multi-scenario ordering with automatic sequential execution

## Quick Start

### Option 1: Desktop Application (Recommended)

Download the application for your platform from the [Releases](../../releases) page and double-click to run.

| Platform | File |
|----------|------|
| macOS (Universal) | `nettopo_test_macos_universal.zip` |
| Windows | `nettopo_test_windows_amd64.zip` |
| Linux | `nettopo_test_linux_amd64.zip` |

### Option 2: CLI Mode

```bash
# Download the appropriate ZIP package for your platform and run:
# Linux:   nettopo_test_cli_linux_amd64.zip
# macOS:   nettopo_test_cli_darwin_amd64.zip or nettopo_test_cli_darwin_arm64.zip
# Windows: nettopo_test_cli_windows_amd64.zip

# Analyze FIO data and generate reports
./nettopo_test_cli -data /path/to/fio/data -output-dir ./output

# Launch the Web management interface
./nettopo_test_cli -web -port 8080
```

### Option 3: Build from Source

```bash
# CLI mode
go build -o nettopo_test_cli ./cmd/cli/

# Desktop mode (requires Wails CLI and Node.js)
go install github.com/wailsapp/wails/v2/cmd/wails@latest
cd frontend && npm install && cd ..
wails build
```

## Project Structure

```
fio-go/
├── cmd/cli/              # CLI entry point
├── main.go               # Wails desktop entry point
├── internal/
│   ├── app/              # Wails bindings
│   ├── executor/         # SSH/FIO remote execution
│   ├── models/           # Data models
│   ├── parser/           # JSON/log parsing
│   ├── report/           # Excel/HTML report generation
│   └── web/              # Web mode server
├── frontend/             # React desktop UI
├── build/                # Build configuration
├── scripts/              # FIO scripts
└── wails.json            # Wails configuration
```

## Usage

### 1. Prepare FIO Data

Organize FIO test JSON and log files in the following structure:

```
data/
├── 192.168.1.100/        # Per-node directories
│   ├── system.txt        # System information
│   └── logs/             # FIO log files
└── 192.168.1.101/
    ├── system.txt
    └── logs/
```

### 2. Generate Reports

```bash
./nettopo_test_cli -data ./data -output-dir ./output
```

Generated files:
- `output/fio_summary.xlsx` — Excel summary (node details, performance overview, merged view)
- `output/fio_report.html` — Interactive HTML report (with ECharts time-series charts)

### 3. Remote Execution (Web/Desktop Mode)

1. Configure FIO test parameters
2. Add target hosts (SSH connection info)
3. Click "Deploy & Run" to push scripts and start tests automatically
4. After tests complete, click "Fetch Data" to download results
5. Click "Generate Report" to analyze and create reports

## Requirements

- Go 1.25+
- Node.js 20+ (only for desktop mode compilation)
- FIO (only on machines running tests)
- SSH access (only for remote execution)

## License

AGPLv3

This project is licensed under the GNU Affero General Public License v3.0 (AGPLv3).

**Key Requirements:**

- Any derivative work based on this project that provides network services or distributes software **must publicly release the complete source code** of both frontend and backend.
- Derivative works include, but are not limited to: frontend pages, backend services, API logic, database scripts, and deployment scripts — all must be fully open-sourced.
- You may not release only the frontend code while keeping the server implementation proprietary.
