# Customized Virtual File System (CVFS)

A user-space virtual file system implementation in C that simulates the core functionalities of a Unix/Linux file system entirely in RAM. This project demonstrates deep understanding of OS-level concepts including **inode management**, **file descriptors**, **superblock architecture**, and **UNIX system call emulation**.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Data Structures](#data-structures)
- [Supported Commands](#supported-commands)
- [How to Build & Run](#how-to-build--run)
- [Usage Examples](#usage-examples)
- [Project Structure](#project-structure)
- [Technical Highlights](#technical-highlights)
- [Future Enhancements](#future-enhancements)

---

## Overview

CVFS (Customized Virtual File System) is an in-memory file system that replicates the behavior of a real operating system's file management subsystem. It implements:

- **Superblock** for tracking total and free inodes
- **Inode Linked List (DILB)** for metadata storage
- **File Table** for managing open file states
- **UFDT (User File Descriptor Table)** for per-process file access

All data resides in memory (RAM), making it a lightweight simulation ideal for understanding OS internals without requiring disk I/O.

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                   User Shell (CLI)                   │
├─────────────────────────────────────────────────────┤
│              Command Parser & Dispatcher             │
├─────────────────────────────────────────────────────┤
│         UFDT (User File Descriptor Table)            │
│         ┌─────────────────────────────┐             │
│         │  File Table Entries          │             │
│         │  (mode, offsets, count)      │             │
│         └──────────┬──────────────────┘             │
│                    │                                 │
│         ┌──────────▼──────────────────┐             │
│         │  Inode List (DILB)           │             │
│         │  (name, size, permissions,   │             │
│         │   buffer, link count)        │             │
│         └──────────────────────────────┘             │
├─────────────────────────────────────────────────────┤
│              Superblock (metadata)                   │
│         TotalInodes: 50 | FreeInodes: N             │
└─────────────────────────────────────────────────────┘
```

---

## Features

- **File Creation** with configurable permissions (Read/Write/Read-Write)
- **File Open/Close** with reference counting
- **Read & Write** operations with offset tracking
- **lseek** — reposition read/write file offset (START, CURRENT, END)
- **File Deletion (rm)** with link count management
- **Truncate** — clear file contents without deleting
- **stat/fstat** — display file metadata (inode number, size, permissions, link count)
- **ls** — list all files with metadata
- **Manual pages (man)** — built-in documentation for each command
- **Cross-platform support** — works on both Windows and Linux

---

## Data Structures

| Structure | Purpose |
|-----------|---------|
| `SUPERBLOCK` | Tracks total and free inode count |
| `INODE` | Stores file metadata (name, size, permissions, buffer, links) |
| `FILETABLE` | Maintains per-open-file state (offsets, mode, reference to inode) |
| `UFDT` | Array of file table pointers — maps file descriptors to file tables |

---

## Supported Commands

| Command | Syntax | Description |
|---------|--------|-------------|
| `create` | `create <filename> <permission>` | Create a new file (1=Read, 2=Write, 3=RW) |
| `open` | `open <filename> <mode>` | Open an existing file |
| `close` | `close <filename>` | Close an opened file |
| `closeall` | `closeall` | Close all opened files |
| `read` | `read <filename> <bytes>` | Read N bytes from file |
| `write` | `write <filename>` | Write data to file |
| `ls` | `ls` | List all files |
| `stat` | `stat <filename>` | Display file info by name |
| `fstat` | `fstat <fd>` | Display file info by file descriptor |
| `truncate` | `truncate <filename>` | Remove all data from file |
| `rm` | `rm <filename>` | Delete a file |
| `lseek` | `lseek <filename> <offset> <origin>` | Change file offset |
| `man` | `man <command>` | Display manual for a command |
| `clear` | `clear` | Clear the console |
| `help` | `help` | Display all available commands |
| `exit` | `exit` | Terminate the file system |

---

## How to Build & Run

### Prerequisites

- **GCC** (MinGW on Windows) or any C compiler

### Compile

```bash
# Linux / macOS
gcc Virthual_File_System.cpp -o cvfs

# Windows (MinGW)
gcc Virthual_File_System.cpp -o cvfs.exe
```

### Run

```bash
# Linux / macOS
./cvfs

# Windows
cvfs.exe
```

---

## Usage Examples

```
Marvellous VFS: > create demo.txt 3
File is successfully created with file descriptor: 0

Marvellous VFS: > write demo.txt
Enter the data:
Hello this is my virtual file system!

Marvellous VFS: > read demo.txt 37
Hello this is my virtual file system!

Marvellous VFS: > stat demo.txt
----------Statistical Information about file----------
File name: demo.txt
Inode number: 1
File size: 37
Link count: 1
Reference count: 1
File permission: Read & Write
------------------------------------------------------

Marvellous VFS: > ls
File Name       Inode Number    File Size       Link Count
-----------------------------------------------
demo.txt        1               37              1
-----------------------------------------------

Marvellous VFS: > rm demo.txt
Marvellous VFS: > exit
Terminating the Marvellous Virtual File System
```

---

## Project Structure

```
Customized_Virtual_File_System_Project/
├── Virthual_File_System.cpp    # Main source file (entire VFS implementation)
└── README.md                   # Project documentation
```

---

## Technical Highlights

- **Inode-based Architecture** — mirrors real Unix/Linux file systems (ext2/ext3)
- **File Descriptor Abstraction** — integer FDs mapping to file table entries
- **Permission Model** — supports Read (1), Write (2), Read+Write (3)
- **Reference & Link Counting** — proper resource management
- **Offset Management** — independent read/write offsets with lseek support
- **Dynamic Memory Allocation** — buffers allocated on demand
- **Custom Shell Interface** — interactive CLI with command parsing

---

## Future Enhancements

- [ ] Directory hierarchy support (nested directories)
- [ ] File copy (`cp`) and move (`mv`) commands
- [ ] Persistence — save/load file system state to disk
- [ ] Multi-user support with user-level permissions (chmod/chown)
- [ ] Backup & restore functionality
- [ ] Logging system for audit trails
- [ ] File search (`find`/`grep`) commands
- [ ] Symbolic and hard link support
- [ ] File system size reporting (`df` command)
- [ ] Tab-completion and command history

---

## Concepts Demonstrated

This project showcases understanding of:

1. **Operating System Internals** — file system layers, system calls
2. **Data Structures** — linked lists, arrays, dynamic memory
3. **Memory Management** — malloc/free, buffer management
4. **Systems Programming** — low-level C programming, pointer manipulation
5. **Software Design** — modular functions, error handling, CLI design

---

## Author

Kaivallya Titame

---

## License

This project is for educational purposes.