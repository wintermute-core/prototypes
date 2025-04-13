# RIK Prototype

A tool for deploying and managing Kubernetes jobs with file uploading capabilities.

## Overview

RIK (Remote Installation Kit) is a utility that streamlines the process of deploying Kubernetes jobs and uploading local files to them. It handles the deployment lifecycle, file packaging, transfer, and initialization signaling.

## Workflow

1. Installs/upgrades a Helm chart which schedules a single-run job with an init container and application container
2. Waits for the job pod to be scheduled and reach the init state
3. Packages and uploads the specified local directory to the init container's volume
4. Sets a flag indicating content upload is complete
5. Init container exits, allowing the main application container to start

## Usage

```bash
# Basic usage with default values
./rik-sh

# With custom name and namespace
NAME=app1 NAMESPACE=dev ./rik-sh

# With custom Helm chart
NAME=app1 NAMESPACE=dev HELM_CHART=https://github.com/wintermute-core/prototypes/releases/download/rik-helm-0.0.1/rik-0.1.0.tgz ./rik-sh

# Enable debug mode for verbose output
DEBUG=true NAME=app1 NAMESPACE=dev ./rik-sh

# Custom upload directory and pattern
NAME=app1 NAMESPACE=dev UPLOAD_DIR=/path/to/files UPLOAD_PATTERN="*.json" ./rik-sh

# Set custom timeout for pod initialization (in seconds)
NAME=app1 NAMESPACE=dev MAX_WAIT_TIME=600 ./rik-sh
```

## Configuration Options

| Environment Variable | Description                                           | Default Value     |
|----------------------|-------------------------------------------------------|-------------------|
| `NAME`               | Application name                                      | `etlapp`          |
| `NAMESPACE`          | Kubernetes namespace                                  | `dev`             |
| `HELM_CHART`         | Helm chart to install                                 | `helm`            |
| `UPLOAD_DIR`         | Directory containing files to upload                  | Current directory |
| `UPLOAD_PATTERN`     | Pattern of files to include in the upload             | `.` (everything)  |
| `MAX_WAIT_TIME`      | Maximum time to wait for pod initialization (seconds) | `300` (5 minutes) |
| `DEBUG`              | Enable verbose debugging output                       | `false`           |
| `JOBNAME`            | Custom job name pattern for pod detection             | Same as `NAME`    |

## Features

- **Automatic Cleanup**: Temporary files are automatically removed when the script completes or encounters an error
- **Error Handling**: Comprehensive error detection and reporting for each operation
- **Timeouts**: Configurable timeout for pod initialization to prevent hanging
- **Logging**: Structured logging with timestamps and log levels for better visibility
- **Exit Codes**: Non-zero exit codes for failure scenarios to support automation

## Requirements

- `kubectl` with access to the target Kubernetes cluster
- `helm` (v3+)
- Bash shell environment

## Example Values File

The script expects a values file named `[NAME]-values.yaml` in the current directory. Example:

```yaml
# app1-values.yaml
image:
  repository: myregistry/myapp
  tag: latest
resources:
  limits:
    cpu: 1
    memory: 1Gi
```

## Troubleshooting

- If the script fails with "pod not found" errors, check that the `JOBNAME` pattern matches your actual pod name
- For upload issues, ensure the Kubernetes cluster has sufficient storage and the init container has proper permissions
- Use `DEBUG=true` to see detailed execution information for troubleshooting