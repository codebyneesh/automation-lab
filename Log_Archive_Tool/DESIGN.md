# Design Overview

## Goals

- Build a CLI tool to archive log files
- Accept a user-provided log directory
- Compress `.log` files into a timestamped archive
- Store the archive in a new directory
- Keep behavior predictable and safe

## Non-Goals (v1)

- Recursive log discovery
- Direct automation of `/var/log`
- Interactive prompts

## CLI Interface

### Required

- `log-archive <log_directory>`

### Optional

- `log-archive <log_directory> <destination_directory>`
- `log-archive -h | --help`

### Planned (v2)

- `--recursive` to include subdirectories

## Archive Strategy

- Match files ending in `.log`
- Archive format: `.tar.gz`
- Naming convention:
  - `logs_archive_YYYY-MM-DD_HHMMSS.tar.gz`
- Timestamp format chosen for natural sorting

## Default Destination

- If no destination is provided, use `/tmp`

## Error Handling

- **Destination not writable**
  - Message:
    - `Default location (/tmp) is not writable. Please choose a new destination. Use --help for information`
  - Exit with non-zero code

- **Destination does not exist**
  - Attempt to create directory
  - If permission denied:
    - Message: `Permission denied: <destination>`
    - Exit with non-zero code

- **Insufficient disk space**
  - Message:
    - `Not enough space in <destination>. Please choose a new destination`
  - Exit with non-zero code

- **No log files found**
  - Message:
    - `No log files exist in this directory`
  - Exit with code `0`

## Future Enhancements

- Recursive log discovery
- Support for `/var/log`
- Configurable file patterns
