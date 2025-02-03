# Imposter GitHub Actions [![Test Actions](https://github.com/imposter-project/imposter-github-action/actions/workflows/test.yml/badge.svg)](https://github.com/imposter-project/imposter-github-action/actions/workflows/test.yml)

This repository contains official GitHub Actions for [Imposter](https://www.imposter.sh), a modern mock server designed for microservice development and testing. These actions allow you to seamlessly integrate Imposter into your GitHub Actions workflows.

## Available Actions

### 1. Setup Imposter (`setup`)
Downloads and installs the Imposter mock server.

```yaml
- uses: imposter-project/imposter-github-action/setup@v1
```

### 2. Start Mocks (`start-mocks`)
Starts the Imposter mock server in the background, and waits for it to be ready.

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
```

<details>
<summary>Advanced configuration options</summary>

- `max-attempts`: Maximum number of attempts to check if the server is ready (default: 30)
- `retry-interval`: Interval in seconds between retry attempts (default: 1)

</details>

### 3. Stop Mocks (`stop-mocks`)
Stops the running Imposter mock server.

```yaml
- uses: imposter-project/imposter-github-action/stop-mocks@v1
  with:
    # Optional: Type of mock engine to use (jvm or docker)
    engine-type: 'docker'      # default: 'docker'
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
        engine-type: 'docker'
        version: '1.2.3'        # Optional: specify engine version
    
    # Your test steps here
    - name: Run Tests
      run: |
        # Add your test commands here
        echo "Running tests against mock server..."
    
    # Stop mock server
    - name: Stop Mocks
      uses: imposter-project/imposter-github-action/stop-mocks@v1
      with:
        engine-type: 'docker'   # Should match the engine-type used in start-mocks
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
