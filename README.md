# actions-bbr

This action attempts to enable the BBR (Bottleneck Bandwidth and RTT) congestion control algorithm on your GitHub Actions runners to potentially improve network performance. It supports both Linux and Windows platforms.

## What it does

- On **Linux**: Enables BBR congestion control algorithm and sets the Fair Queue (FQ) qdisc
- On **Windows**: Enables BBR2 congestion control algorithm for all TCP profiles

> [!NOTE]
> **The BBR2 congestion control algorithm is ONLY available on Windows Server 2025 and newer versions.** 
> On older Windows versions, the action will exit gracefully without making changes.

## Usage

```yaml
name: My Workflow

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest # or windows-2025
    
    steps:
    - uses: actions/checkout@v4
    
    # Enable BBR congestion control early in your workflow
    - name: Enable TCP BBR
      uses: Zxilly/actions-bbr@v1
      
    # Your other workflow steps
    - name: Build and Test
      run: |
        # Your commands
```

## How It Works

### Linux Implementation
- Loads the `tcp_bbr` kernel module
- Sets `net.core.default_qdisc=fq`
- Sets `net.ipv4.tcp_congestion_control=bbr`
- Verifies the changes

### Windows Implementation
- Checks if BBR2 is available as a congestion provider option
- If available, applies BBR2 to all TCP setting profiles
- Verifies the changes

## Requirements

- **Linux**: Kernel version 4.9 or newer with BBR module support
- **Windows**: Windows Server 2025 or newer for BBR2 support

## License

MIT