# SQL Table Exporter

A high-performance .NET utility for exporting large SQL Server tables to CSV files with built-in pagination, restartability, and schema script generation.

## Features

- **Efficient Pagination**: Uses keyset pagination for optimal performance with large tables
- **Chunked Export**: Breaks exports into multiple files with configurable row limits
- **Progress Tracking**: Monitors export progress with ETA and performance metrics
- **Restart Capability**: Resume interrupted exports from the last saved position
- **Schema Script Generation**: Automatically creates table schema scripts
- **Primary Key Detection**: Automatically detects and uses primary keys for ordering

## Requirements

- .NET 9.0 or later
- Windows operating system
- SQL Server database access
- Appropriate permissions to read from the tables you want to export

## Installation

Download the SQLTableExporter executable from the releases page

## Usage

```bash
SQLTableExporter [OPTIONS] [OUTPUT_DIRECTORY]
```

### Basic Examples

Export a table with default settings:
```bash
SQLTableExporter -c "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;" -s dbo -t Users --order-by "Id"
```

Export to a specific directory:
```bash
SQLTableExporter -c "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;" -s dbo -t Stats --order-by "User" -o ./exports
```

Specify connection string:
```bash
SQLTableExporter -c "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;" -s dbo -t Products -o ./exports
```

Customize export parameters:
```bash
SQLTableExporter -c "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;" -s dbo -t Orders --order-by "OrderDate, OrderId" --rows 500000 ./exports
```

Resume a previously interrupted export:
```bash
SQLTableExporter -c "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;" -s dbo -t LargeTable --restart
```

## Command-Line Options

| Option                   | Description                                             | Default        |
|--------------------------|---------------------------------------------------------|----------------|
| `-c, --connection`       | Connection string to SQL database                       | Required       |
| `-o, --output`           | Output directory for CSV files                          | Current dir    |
| `-s, --schema`           | Database schema name                                    | Required       |
| `-t, --table`            | Table name to export                                    | Required       |
| `--order-by`             | Column(s) to order by during export                     | Primary key    |
| `-p, --prefix`           | Prefix for output CSV files                             | Table name     |
| `-r, --rows`             | Maximum rows per CSV file                               | 1,000,000      |
| `-b, --batch-size`       | Number of rows to fetch per query                       | 5,000          |
| `-d, --delay`            | Delay in milliseconds between query batches             | 10             |
| `--timeout`              | Command timeout in seconds                              | 3,600          |
| `--restart`              | Restart from previous export progress                   | False          |
| `--no-progress-tracking` | Disable progress tracking                               | False          |
| `--no-schema-script`     | Disable schema script generation                        | False          |

## Self-Management Commands

These commands are used to manage the application itself:

| Command                  | Description                                                    |
|--------------------------|----------------------------------------------------------------|
| `update`                 | Checks for a newer version and initiates update if available   |
| `version`                | Displays the current version of the application                |

### Update Command

```bash
SQLTableExporter update
```

When executed, this command checks if a newer version is available:
- If an update is found, it displays the new version number and automatically starts the update process
- If no update is available, it reports "No new updates"
- If the update check fails, it reports an error

### Version Command

```bash
SQLTableExporter version
```

Displays the current version of SQLTableExporter in the format of `Major.Minor`.

## Output Files

The tool produces the following files:

- **CSV Files**: `{prefix}_{number}.csv` - Each file contains up to the specified maximum rows
- **Schema Script**: `{prefix}_schema.sql` - SQL script to recreate the table structure
- **Progress File**: `{schema}_{table}_export_progress.json` - Used for tracking and restarting exports

## Advanced Features

### Keyset Pagination

The exporter uses keyset pagination for optimal performance, which is significantly more efficient than OFFSET/FETCH for large tables. It works by tracking the values of the ordering columns in the last row of each batch and using them to fetch the next batch.

### Progress Tracking

During export, the tool provides detailed progress information:
- Percentage complete
- Current file number
- Elapsed and estimated remaining time
- Export throughput (rows/second)

### Restart Capability

The `--restart` option allows you to resume an interrupted export. The tool saves progress after each file is completed, including the values of the ordering columns from the last processed row. This allows for precise resumption without duplicates or gaps.

### Schema Script Generation

The tool can generate a comprehensive SQL script that recreates the table structure, including:
- Column definitions with correct data types and nullability
- Primary keys
- Foreign keys with referential actions
- Indexes
- Check constraints

## Performance Considerations

- **Batch Size**: Adjust the `-b, --batch-size` parameter based on your table structure. Smaller batches reduce memory usage but increase the number of queries.
- **Delay**: The `-d, --delay` parameter adds a configurable delay between query batches to reduce server load.
- **Order By**: Select columns with high cardinality for ordering to optimize pagination performance.

## Error Handling

The tool has built-in error handling for common issues:
- Connection problems
- Permission issues
- Timeout errors
- Data type conversion challenges

If an error occurs, the tool will display the error message and stack trace. When using the restart feature, it will resume from the last successfully exported file.
