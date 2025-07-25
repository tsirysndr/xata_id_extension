# xata_id PostgreSQL Extension
[![ci](https://github.com/tsirysndr/xata_id_extension/actions/workflows/ci.yml/badge.svg)](https://github.com/tsirysndr/xata_id_extension/actions/workflows/ci.yml)

A PostgreSQL extension written in Rust using pgrx to generate Xata-style unique identifiers (xata_id) in the format rec_<20_random_chars>. The IDs consist of a rec_ prefix followed by a 20-character random string using the alphabet a-z0-9, mimicking Xata's ID format (e.g., ` rec_cug4h6ibhfbm7uq5dte0`).

## Features
- Generates 24-character unique IDs with a rec_ prefix and 20 random characters (a-z0-9).
- Safe integration with PostgreSQL via pgrx for robust memory management.
- Comprehensive unit and integration tests to verify ID format, length, and uniqueness.
- Lightweight and performant, suitable for use as a primary key default value.

## Requirements
- PostgreSQL 13–18
- Rust and cargo-pgrx (version 0.15)
- A compatible operating system (Linux, macOS, or Windows with WSL)

## Installation
**1. Install Dependencies**

Install Rust and pgrx:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
cargo install cargo-pgrx
cargo pgrx init
```

**2. Clone the repository:**

```bash
git clone https://github.com/tsirysndr/xata_id_extension
cd xata_id_extension
```

**3. Build the Extension**

Compile the extension for your PostgreSQL version (e.g., PostgreSQL 16):

```bash
cargo pgrx package
```

**4. Install the Extension**

Install the extension to your PostgreSQL instance:

```bash
cargo pgrx install
```

**5. Enable the Extension in PostgreSQL**

Connect to your PostgreSQL database and run:

```bash
psql -d your_database_name
```

Then, enable the extension:

```sql
CREATE EXTENSION xata_id_extension;
```

## Usage

The extension provides a `xata_id()` function that generates a unique ID. Use it as the default value for a primary key in a table:

**Create a Table**

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY DEFAULT xata_id(),
    username VARCHAR(50) NOT NULL,
    email VARCHAR(255) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
**Insert Data**

Insert rows, and the id column will automatically use xata_id():

```sql
INSERT INTO users (username, email) VALUES ('alice', 'alice@example.com');
INSERT INTO users (username, email) VALUES ('bob', 'bob@example.com');
SELECT * FROM users;
```

**Example Output**

```plaintext
 id                       | username | email             | created_at
--------------------------+----------+-------------------+------------------------
 rec_cug4h6ibhfbm7uq5dte0 | alice    | alice@example.com | 2025-07-25 12:00:00
 rec_4h6ibhfbm7uq5dte0cug | bob      | bob@example.com   | 2025-07-25 12:01:00
```

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.