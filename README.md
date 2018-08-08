## SQLite CREATE TABLE Parser
A parser for sqlite create table sql statements.

[SQLite](https://www.sqlite.org/) is a very powerful software but it lacks an easy way to extract complete information about tables and columns constraints. The built-in sql pragma:  
```c
PRAGMA schema.table_info(table-name);  
PRAGMA foreign_key_list(table-name);
```  
provide incomplete information and a manual parsing is required in order to extract more useful information.

CREATE TABLE syntax diagrams can be found on the official [sqlite website](https://www.sqlite.org/lang_createtable.html).

## Usage
In order to extract the original CREATE TABLE sql statement you need to query the sqlite_master table
```sql
SELECT sql FROM sqlite_master WHERE name = 'myTable';
```

then just include sql3parse_table.h and sql3parse_table.c in your project and invoke:
```c
// sql is the CREATE TABLE sql statement
// length is the length of sql (if 0 then strlen will be used)
// error is the returned error code (can be NULL)

sql3table *sql3parse_table (const char *sql, size_t length, sql3error_code *error);
```
**sql3table** is an opaque struct that you can introspect using the sql3table* public functions.  

## API
To Do

## ALTER TABLE
To Do

## Speed and memory considerations
The parser is blazing fast, mainly because very few memory allocations are performed and no copy operations are used between the sql string and the internal sql3string structs. Memory requirement is linearly proportional to the number of columns in the table.
```
You can estimate memory usage (on a 64bit system) usign the following formula:
N1: number of columns WITHOUT a foreign key constraint
N2: number of columns WITH a foreign key constraint
N3: number of indexed columns in table constraint
K: 0 if no foreign key yable constraint is used or 64
Memory usage (in bytes): 144 + (N1 * 144) + (N2 * 208) + (N3 * 40) + K
```

## Pre-requisites
- A C99 compiler.
- SQL statement must be successfully compiled by sqlite.
