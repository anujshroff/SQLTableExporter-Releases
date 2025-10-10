## v1.6
Improvements:
- Updated dependencies

## v1.5
Improvements:
- Updated dependencies

## v1.4
New Features:
- Optional snapshot isolation mode (--snapshot-isolation)

## v1.3
Bug Fixes:
- Fix progress tracking during post export phases

## v1.2
New Features:
- Add optional where condition parameterized input
- Add optional archive feature
- Add optional Azure blob storage upload feature
  - DefaultAzureCredential
  - SAS URL

Improvements:
- Use proper exit codes for success and failure

## v1.1
Improvements:
- Minor code reorganization. No notable changes.

## v1.0
New Features:
- Export SQL tables to CSV files
- Auto order by primary key detection
- Auto schema detection
- Generates SQL script to recreate table
- Tracks progress for restarting export
- Configurable batch sizes, max row count per file, delay between batches, columns to order by, and command timeout