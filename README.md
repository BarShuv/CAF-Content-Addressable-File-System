# CAF - Content-Addressable File System

A Git-like version control system implemented in Python and C++, featuring content-addressable storage, branch management, commit history, and diff capabilities.

## Team Members
- Omer ben arie
- Meshi levi  
- Bar shuv

## Overview

CAF (Content-Addressable File System) is a lightweight version control system that provides core Git-like functionality. It uses content-addressable storage where each file and directory is stored based on its cryptographic hash, ensuring data integrity and efficient deduplication.

### Key Features

- **Repository Initialization**: Create new CAF repositories with customizable default branches
- **File Hashing**: SHA-256-based content addressing for files
- **Commit Management**: Create commits with author information and messages
- **Branch Operations**: Create, delete, and list branches
- **Commit History**: View the complete history of commits (log)
- **Diff Functionality**: Compare changes between commits, including file moves detection
- **Tree Storage**: Efficient directory tree storage and retrieval
- **High-Performance Core**: C++ implementation for performance-critical operations

## Architecture

The project consists of two main components:

### 1. `libcaf` - Core Library
- **Language**: C++ with Python bindings (using pybind11)
- **Purpose**: High-performance implementation of core version control operations
- **Key Components**:
  - Content-addressable storage (objects directory)
  - File hashing (SHA-256)
  - Blob, Tree, and Commit object handling
  - Object serialization and deserialization

### 2. `caf` - Command-Line Interface
- **Language**: Python
- **Purpose**: User-facing CLI application
- **Features**: Full command-line interface for all version control operations

## Prerequisites

- **Docker**: The project uses Docker for a consistent development environment
- **Docker Buildx**: Required for building the Docker image
- **Make**: For running project commands

Alternatively, for local development without Docker:
- **Python**: 3.10 or higher
- **GCC/G++**: For compiling C++ code
- **OpenSSL**: Development libraries (libssl-dev)
- **pybind11**: For Python-C++ bindings

## Installation

### Using Docker (Recommended)

1. **Clone the repository**:
   ```bash
   git clone https://github.com/BarShuv/2025a-team1.git
   cd 2025a-team1
   ```

2. **Build and run the Docker container**:
   ```bash
   make build
   make run
   ```

3. **Deploy the application**:
   ```bash
   make deploy
   ```

   This will:
   - Compile the C++ library (`libcaf`)
   - Install both `libcaf` and `caf` packages

4. **Attach to the container** (optional):
   ```bash
   make attach
   ```

### Manual Installation (Without Docker)

1. **Install system dependencies**:
   ```bash
   # Ubuntu/Debian
   sudo apt-get update
   sudo apt-get install -y gcc g++ make cmake python3 python3-dev python3-pip libssl-dev
   
   pip install pybind11 setuptools pytest
   ```

2. **Install libcaf**:
   ```bash
   cd libcaf
   python setup.py develop
   pip install -e .
   cd ..
   ```

3. **Install caf CLI**:
   ```bash
   cd caf
   pip install -e .
   cd ..
   ```

## Usage

### Basic Commands

#### Initialize a Repository
```bash
caf init [--working_dir_path PATH] [--repo_dir .caf] [--default_branch main]
```

#### Hash a File
```bash
# Print the hash of a file
caf hash_file <file_path>

# Hash and save the file to the repository
caf hash_file <file_path> -w
```

#### Create a Commit
```bash
caf commit --author "Your Name" --message "Commit message"
```

#### Branch Management
```bash
# List all branches
caf branch

# Create a new branch
caf add_branch <branch_name>

# Check if a branch exists
caf branch_exists <branch_name>

# Delete a branch
caf delete_branch <branch_name>
```

#### View Commit History
```bash
caf log
```

#### Compare Commits
```bash
caf diff <commit_hash1> <commit_hash2>
```

#### Delete Repository
```bash
caf delete_repo
```

### Example Workflow

```bash
# Initialize a new repository
caf init

# Hash and save a file
echo "Hello, CAF!" > test.txt
caf hash_file test.txt -w

# Create a commit
caf commit --author "John Doe" --message "Initial commit"

# Create a new branch
caf add_branch feature-branch

# View commit history
caf log

# Make changes and create another commit
echo "More content" >> test.txt
caf commit --author "John Doe" --message "Added more content"

# Compare commits
caf diff <first_commit_hash> <second_commit_hash>
```

## Development

### Project Structure

```
.
├── caf/                    # CLI application
│   ├── caf/
│   │   ├── __main__.py    # Entry point
│   │   ├── cli.py         # CLI argument parsing
│   │   └── cli_commands.py # Command implementations
│   └── pyproject.toml
│
├── libcaf/                 # Core library
│   ├── libcaf/            # Python package
│   │   ├── __init__.py
│   │   ├── constants.py
│   │   └── repository.py  # Repository operations
│   ├── src/               # C++ source code
│   │   ├── caf.cpp        # Core C++ implementation
│   │   ├── bind.cpp       # Python bindings
│   │   ├── object_io.cpp  # Object I/O operations
│   │   └── *.h            # Header files
│   ├── setup.py
│   └── pyproject.toml
│
├── tests/                  # Test suite
│   ├── caf/               # CLI tests
│   └── libcaf/            # Library tests
│
├── Dockerfile             # Development environment
├── Makefile               # Build automation
└── README.md
```

### Building the Project

#### Using Make (Docker)
```bash
# Build Docker image
make build

# Run container
make run

# Compile C++ library
make compile

# Deploy all components
make deploy

# Run all tests
make test

# Run only libcaf tests
make test_libcaf

# Run only CLI tests
make test_caf
```

### Testing

The project uses pytest for testing:

```bash
# Run all tests (in Docker)
make test

# Run tests for libcaf only
make test_libcaf

# Run tests for CLI only
make test_caf

# Run tests locally (without Docker)
pytest tests/
```

### Cleaning Up

```bash
# Stop the container
make stop

# Remove the container and generated files
make remove

# Clean Docker resources and build artifacts
make clean
```

## Repository Storage Format

CAF uses a content-addressable storage model similar to Git:

- **`.caf/`**: Main repository directory
  - **`objects/`**: Stores all content-addressed objects (blobs, trees, commits)
  - **`refs/heads/`**: Branch references
  - **`HEAD`**: Points to the current branch or commit

Objects are stored using SHA-256 hashing, where the first 2 characters of the hash form a subdirectory name, and the remaining characters form the filename.

## Technical Details

- **Hashing Algorithm**: SHA-256
- **Language**: Python 3.10+ with C++ extensions
- **C++ Standard**: C++17
- **Python Bindings**: pybind11
- **Testing Framework**: pytest
- **Cryptography**: OpenSSL (for SHA-256)

## License

[Specify license here if applicable]

## Contributing

[Add contributing guidelines if applicable]

## Help

For a list of all available commands:
```bash
caf --help
```

For help with a specific command:
```bash
caf <command> --help
```

## Troubleshooting

### Common Issues

1. **Docker buildx not found**: Run `make buildx-check` to install it automatically
2. **Container already running**: Use `make stop` then `make run`
3. **Permission issues with files/objects**: The Makefile includes sudo commands for cleanup

For more information or to report issues, please contact the team members listed above.
