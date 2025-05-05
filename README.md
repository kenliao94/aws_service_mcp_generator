# AWS Service MCP Generator

A Python library that automatically generates MCP (Machine Conversation Protocol) servers for any AWS service that has a boto3 client. This tool simplifies the process of creating AI-powered interfaces for AWS services.

## Installation

You can install the package using either pip or uv:

### Using pip

```bash
pip install aws_service_mcp_generator
```

### Using uv

```bash
uv add aws_service_mcp_generator
```

For development installation:

```bash
# Clone the repository
git clone https://github.com/kenliao94/aws_service_mcp_generator.git
cd aws_service_mcp_generator

# Create and activate a virtual environment (optional but recommended)
uv venv --python 3.10
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install the package in development mode
uv pip install -e .
```

## Usage

The AWS Service MCP Generator allows you to quickly create MCP servers for any AWS service. Here's how to use it:

### Basic Usage

```python
from mcp.server.fastmcp import FastMCP
from aws_service_mcp_generator.generator import AWSToolGenerator


# Initialize FastMCP server
mcp = FastMCP()

# Create a generator for an AWS service
sns_generator = AWSToolGenerator(
    service_name="sns",
    service_display_name="Simple Notification Service",
    mcp=mcp
)

# Generate MCP tools for all operations
sns_generator.generate()

# Start the MCP server
mcp.run()
```

### Advanced Configuration

You can customize the behavior of specific operations:

```python
from mcp.server.fastmcp import FastMCP
from aws_service_mcp_generator.generator import AWSToolGenerator

mcp = FastMCP()

# Configure specific operations
tool_configuration = {
    "list_topics": {
        "documentation_override": "List all SNS topics in the specified region"
    },
    "create_topic": {
        "validator": lambda mcp, client, kwargs: (
            True if kwargs.get("Name") else (False, "Topic name is required")
        )
    },
    "delete_topic": {
        "ignore": True  # Skip this operation
    }
}

# Create generator with custom configuration
sns_generator = AWSToolGenerator(
    service_name="sns",
    service_display_name="Simple Notification Service",
    mcp=mcp,
    tool_configuration=tool_configuration
)

sns_generator.generate()
mcp.run()
```

## Contributing

Contributions are welcome! Here's how you can contribute to this project:

1. **Fork the Repository**
   - Fork the repository on GitHub

2. **Clone Your Fork**
   ```bash
   git clone https://github.com/your-username/aws_service_mcp_generator.git
   cd aws_service_mcp_generator
   ```

3. **Set Up Development Environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   pip install -e .
   uv add --dev pyright ruff
   ```

4. **Create a Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

5. **Make Your Changes**
   - Implement your feature or bug fix
   - Add or update tests as necessary
   - Update documentation if needed

6. **Run Tests**
   ```bash
   pytest
   ```

7. **Run Linters**
   ```bash
   ruff check .
   ```

8. **Commit Your Changes**
   ```bash
   git commit -m "Add feature: your feature description"
   ```

9. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

10. **Submit a Pull Request**
    - Go to the original repository on GitHub
    - Click "New Pull Request"
    - Select your fork and branch
    - Fill in the PR template with details about your changes

### Code Style

This project follows these coding standards:
- Line length: 99 characters
- Quote style: Single quotes
- Python version: 3.10+
- Use type hints

### Running Tests

```bash
pytest
```

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.