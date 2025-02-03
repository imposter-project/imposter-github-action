# Imposter GitHub Actions [![Test Actions](https://github.com/imposter-project/imposter-github-action/actions/workflows/test.yml/badge.svg)](https://github.com/imposter-project/imposter-github-action/actions/workflows/test.yml)

This repository contains official GitHub Actions for [Imposter](https://www.imposter.sh), a modern mock server designed for microservice development and testing. These actions allow you to seamlessly integrate Imposter into your GitHub Actions workflows.

## Available Actions

### 1. Setup Imposter (`setup`)
Downloads and installs the Imposter CLI tool.

```yaml
- uses: imposter-project/imposter-github-action/setup@v1
```

### 2. Start Mocks (`start-mocks`)
Starts the Imposter mock server in the background.

```yaml
- uses: imposter-project/imposter-github-action/start-mocks@v1
  with:
    # Optional: Port number for the Imposter server (default: 8080)
    port: '8080'
    # Optional: Path to the configuration directory (default: ./mocks)
    config-dir: './mocks'
```

### 3. Stop Mocks (`stop-mocks`)
Stops the running Imposter mock server.

```yaml
- uses: imposter-project/imposter-github-action/stop-mocks@v1
```

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
      uses: imposter-project/imposter-github-action/start-mocks@v1
      with:
        port: '8080'
        config-dir: './mocks'
    
    # Your test steps here
    - name: Run Tests
      run: |
        # Add your test commands here
        echo "Running tests against mock server..."
    
    # Stop mock server
    - name: Stop Mocks
      uses: imposter-project/imposter-github-action/stop-mocks@v1
```

## Configuration

The mock server configuration should be placed in your repository according to the `config-dir` parameter (defaults to `./mocks`). For detailed information about configuring Imposter mocks, please visit the [official documentation](https://www.imposter.sh).

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Links

- [Imposter Official Website](https://www.imposter.sh)
- [Imposter Documentation](https://docs.imposter.sh)
- [GitHub Repository](https://github.com/imposter-project/imposter-github-action)
