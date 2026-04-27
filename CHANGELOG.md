## v1.10
Bug Fixes:
- Fix CSV row split when string columns contain a bare carriage return (CR) — affected fields are now properly quoted
- Fix CSV field boundary issue when string columns contain NUL bytes — affected fields are now properly quoted
- Fix REAL/FLOAT precision loss by using G9/G17 format specifiers instead of "R" (Microsoft's documented round-trip-safe specifiers since .NET 5)
- Fix decimal/numeric columns throwing OverflowException when values exceed .NET decimal range (~29 digits); high-precision values up to SQL Server's full 38-digit range now export correctly
- Fix export crash on hierarchyid, geography, and geometry columns; these CLR types now export as their canonical text form (hierarchyid path string, WKT for spatial types)
- Fix --order-by failing for columns whose names are SQL reserved words (e.g. Order, User); identifiers in WHERE and ORDER BY clauses are now bracket-quoted
- Add warnings when manual --order-by columns are nullable or not covered by a UNIQUE index/constraint, since pagination can skip or duplicate rows in those cases
- Fix pagination and restart for --order-by on a decimal/numeric column whose values exceed .NET decimal range (~29 digits); previously crashed at the first batch boundary
- Fix generated schema script dropping the length specifier on binary, varbinary, and varbinary(MAX) columns; tables recreated from the script were silently sized to 1 byte

## v1.9
Bug Fixes:
- Addressed breaking changes from SharpCompress update
- Fix byte[] columns (timestamp/rowversion, varbinary, binary, image) exporting as the literal "System.Byte[]" instead of their value

## v1.8
Improvements:
- Updated dependencies

## v1.7
Improvements:
- Updated dependencies

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