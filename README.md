# File Hashing Script

## Introduction

The File Hashing script is designed to compute and display the MD5 hash values of all files within a specified directory. It uses the `hashlib` module to generate secure hash values and the `os` module to navigate the directory structure. This script is useful for verifying file integrity and detecting duplicate files based on their hash values.

## Features

- **Calculate MD5 Hash of a File**: Computes the MD5 hash for individual files.
- **Process Entire Directory**: Processes all files in a specified directory and its subdirectories.
- **Error Handling**: Gracefully handles files that cannot be read due to permission issues or other I/O errors.
- **Display Results**: Outputs the MD5 hash and corresponding file path for each file in the directory.

## Prerequisites

- Python 3.6 or higher.

## Modules

- **os**: Provides functions for interacting with the operating system.
- **hashlib**: Contains functions for secure hash and message digest operations.

```python
import os
import hashlib
```

## Functions

### `calculate_md5(file_path)`

This function calculates the MD5 hash of a given file.

- **Parameters:**
  - `file_path` (str): The path to the file whose MD5 hash needs to be calculated.
- **Returns:**
  - `str`: The MD5 hash of the file as a hexadecimal string.
  - `None`: If the file cannot be read due to permission issues or other I/O errors.

#### Example

```python
def calculate_md5(file_path):
    hasher = hashlib.md5()
    try:
        with open(file_path, 'rb') as file:
            buf = file.read()
            hasher.update(buf)
        return hasher.hexdigest()
    except (PermissionError, IOError):
        print(f"Could not read file: {file_path}")
        return None
```

### `process_directory(directory)`

This function processes all files in a specified directory, calculates their MD5 hashes, and stores them in a dictionary.

- **Parameters:**
  - `directory` (str): The path to the directory to be processed.
- **Returns:**
  - `dict`: A dictionary with MD5 hashes as keys and file paths as values.

#### Example

```python
def process_directory(directory):
    fileHashes = {}
    for root, dirs, files in os.walk(directory):
        for fileName in files:
            fullPath = os.path.join(os.path.abspath(root), fileName)
            file_hash = calculate_md5(fullPath)
            if file_hash:
                fileHashes[file_hash] = fullPath
    return fileHashes
```

### `main()`

The main function of the script, which sets the directory to be scanned and processes it.

- Specifies the directory to be scanned.
- Calls `process_directory` to get the file hashes.
- Prints each hash and its corresponding file path.

#### Example

```python
def main():
    directory = "D:\\"
    fileHashes = process_directory(directory)
    for hash, path in fileHashes.items():
        print(f"Hash: {hash}, File: {path}")

if __name__ == "__main__":
    main()
```

## Usage

1. **Set the Directory Path**: Update the `directory` variable in the `main()` function to point to the directory you want to process.

    ```python
    directory = "path_to_your_directory"
    ```

2. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

3. **Output**: The script will print the MD5 hash and corresponding file path for each file in the specified directory.

## Example Output

If the directory contains files such as:

```plaintext
D:\file1.txt
D:\subdir\file2.txt
```

The output will be:

```plaintext
Hash: d41d8cd98f00b204e9800998ecf8427e, File: D:\file1.txt
Hash: 098f6bcd4621d373cade4e832627b4f6, File: D:\subdir\file2.txt
```

## Error Handling

- **File Not Found or Permission Denied**: If a file cannot be read due to permission issues or it does not exist, the script prints an error message and continues processing other files.

```plaintext
Could not read file: D:\restricted_file.txt
```

## Security Considerations

- **Input Validation**: Ensure the directory path provided does not contain sensitive or unauthorized information.
- **Permission Handling**: Make sure the script has appropriate permissions to read files in the specified directory.

## FAQs

**Q: What happens if the directory is empty?**
A: The script will return an empty dictionary and nothing will be printed.

**Q: Can the script process large directories?**
A: Yes, the script uses `os.walk` to iterate through directories and files, making it efficient for large directories.

**Q: How does the script handle different file types?**
A: The script processes all files in binary mode, so it can handle any file type.

## Troubleshooting

- **Invalid Directory Path**: Ensure that the directory path is correct and accessible.
- **Module Not Found Errors**: Make sure Python is installed correctly and all necessary modules are available.

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
