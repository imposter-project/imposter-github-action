# Imposter GitHub Actions [![CI/CD](https://github.com/imposter-project/imposter-github-action/actions/workflows/ci.yml/badge.svg)](https://github.com/imposter-project/imposter-github-action/actions/workflows/ci.yml)

This repository contains official GitHub Actions for [Imposter](https://www.imposter.sh), a modern mock server designed for microservice development and testing. These actions allow you to seamlessly integrate Imposter into your GitHub Actions workflows.

> [!NOTE]
> Replace `v1` in the examples below with the [latest release](https://github.com/imposter-project/imposter-github-action/releases).

## Available Actions

### 1. Setup Imposter (`setup`)
Downloads and installs the Imposter mock server.

```yaml
- uses: imposter-project/imposter-github-action/setup@v1
```

### 2. Start Mocks (`start-mocks`)
Starts the Imposter mock server in the background. The step returns only once the server is healthy, so the next step can hit it straight away.

```yaml
- uses: imposter-project/imposter-github-action/start-mocks@v1
  with:
    # Optional: Path to the directory containing the Imposter configuration files
    config-dir: './mocks'      # default: './mocks'
    # Optional: Port number for the Imposter server
    port: '8080'               # default: '8080'
    # Optional: Version of the Imposter mock engine to use
    version: '1.2.3'           # default: '' (latest)
    # Optional: Type of mock engine to use (jvm or docker)
    engine-type: 'docker'      # default: 'docker'
    # Optional: Whether to recursively scan the config directory
    recursive-config-scan: 'false'  # default: 'false'
    # Optional: Write the started mock's ID to this file, to stop it later by ID
    mock-id-file: 'mock-id.txt'     # default: '' (don't write an ID file)
```

> [!NOTE]
> For `version`, choose from the Imposter [releases](https://github.com/outofcoffee/imposter/releases) page.

#### Outputs
- `base-url`: Base URL of the mock server (e.g. `http://localhost:8080`)

### 3. Stop Mocks (`stop-mocks`)
Stops a specific mock by ID, or every running mock. With no inputs, it stops them all.

```yaml
- uses: imposter-project/imposter-github-action/stop-mocks@v1
  with:
    # Optional: Stop one specific mock by its ID
    mock-id: 'abc123'              # default: '' (stop all mocks)
    # Optional: Stop the mock whose ID was written to this file by start-mocks
    mock-id-file: 'mock-id.txt'    # default: '' (stop all mocks)
```

> [!NOTE]
> Provide at most one of `mock-id` or `mock-id-file`. The `engine-type` input is deprecated and ignored.

## Sample Workflow

Here's a complete example showing how to use all three actions in a workflow:

```yaml
name: Integration Tests with Mocks

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    # Install Imposter CLI
    - name: Setup Imposter
      uses: imposter-project/imposter-github-action/setup@v1
    
    # Start mock server
    - name: Start Mocks
      id: start-mocks
      uses: imposter-project/imposter-github-action/start-mocks@v1
      with:
        config-dir: './mocks'
        port: '8080'            # Optional: specify port number
        engine-type: 'docker'   # Optional: specify engine type
        version: '1.2.3'        # Optional: specify engine version
        recursive-config-scan: 'true'  # Optional: scan config directory recursively
        mock-id-file: 'mock-id.txt'    # Optional: record the mock ID to stop it later
    
    # Your test steps here
    - name: Run Tests
      run: |
        # The mock server is available at ${{ steps.start-mocks.outputs.base-url }}
        echo "Running tests against mock server at ${{ steps.start-mocks.outputs.base-url }}"
    
    # Stop mock server (the exact one we started)
    - name: Stop Mocks
      uses: imposter-project/imposter-github-action/stop-mocks@v1
      with:
        mock-id-file: 'mock-id.txt'
```

## Configuration

The mock server configuration should be placed in your repository according to the `config-dir` parameter (defaults to `./mocks`). For detailed information about configuring Imposter mocks, please visit the [official documentation](https://docs.imposter.sh/configuration/).

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Links

- [Imposter Official Website](https://www.imposter.sh)
- [Imposter Documentation](https://docs.imposter.sh)
- [GitHub Repository](https://github.com/imposter-project/imposter-github-action)
