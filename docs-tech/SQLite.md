TITLE: Compiling SQLite on Unix-like Systems
DESCRIPTION: Step-by-step commands to compile SQLite on Unix-like systems, including installing dependencies, unpacking source code, configuring the build environment, and building various targets.
SOURCE: https://github.com/sqlite/sqlite/blob/master/README.md#2025-04-22_snippet_2

LANGUAGE: shell
CODE:
```
apt install gcc make tcl-dev  ;#  Make sure you have all the necessary build tools
tar xzf sqlite.tar.gz         ;#  Unpack the source tree into "sqlite"
mkdir bld                     ;#  Build will occur in a sibling directory
cd bld                        ;#  Change to the build directory
../sqlite/configure           ;#  Run the configure script
make sqlite3                  ;#  Builds the "sqlite3" command-line tool
make sqlite3.c                ;#  Build the "amalgamation" source file
make sqldiff                  ;#  Builds the "sqldiff" command-line tool
# Makefile targets below this point require tcl-dev
make tclextension-install     ;#  Build and install the SQLite TCL extension
make devtest                  ;#  Run development tests
make releasetest              ;#  Run full release tests
make sqlite3_analyzer         ;#  Builds the "sqlite3_analyzer" tool
```

----------------------------------------

TITLE: Building SQLite Components Requiring TCL
DESCRIPTION: Make commands for building SQLite components and running tests that require TCL. These targets depend on the TCL installation.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-unix.md#2025-04-22_snippet_3

LANGUAGE: bash
CODE:
```
make tclextension-install
```

LANGUAGE: bash
CODE:
```
make devtest
```

LANGUAGE: bash
CODE:
```
make releasetest
```

LANGUAGE: bash
CODE:
```
make sqlite3_analyzer
```

----------------------------------------

TITLE: Generating Series Virtual Table in C for SQLite
DESCRIPTION: The series.c file demonstrates an implementation of the 'generate_series' virtual table for SQLite, useful as a pattern for developing custom virtual tables.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/misc/README.md#2025-04-22_snippet_6

LANGUAGE: C
CODE:
```
// Generate series virtual table
// Template for custom virtual tables
#include <sqlite3ext.h>
// Additional C code implementation...
```

----------------------------------------

TITLE: Building SQLite DLL with Specific Features using nmake
DESCRIPTION: Builds the SQLite DLL (`sqlite3.dll`) and its corresponding definition file (`sqlite3.def`) using `nmake` and `Makefile.msc`. The `USE_NATIVE_LIBPATHS=1` option is specified, and the `OPTS` parameter provides a string of C preprocessor defines (`-D...`) to enable various optional features like FTS3, FTS4, FTS5, RTREE, JSON1, GEOPOLY, SESSION, PREUPDATE_HOOK, SERIALIZE, and MATH_FUNCTIONS.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-windows.md#2025-04-22_snippet_13

LANGUAGE: cmd
CODE:
```
nmake /f Makefile.msc sqlite3.dll USE_NATIVE_LIBPATHS=1 "OPTS=-DSQLITE_ENABLE_FTS3=1 -DSQLITE_ENABLE_FTS4=1 -DSQLITE_ENABLE_FTS5=1 -DSQLITE_ENABLE_RTREE=1 -DSQLITE_ENABLE_JSON1=1 -DSQLITE_ENABLE_GEOPOLY=1 -DSQLITE_ENABLE_SESSION=1 -DSQLITE_ENABLE_PREUPDATE_HOOK=1 -DSQLITE_ENABLE_SERIALIZE=1 -DSQLITE_ENABLE_MATH_FUNCTIONS=1"
```

----------------------------------------

TITLE: Implementing In-Memory VFS for SQLite in C
DESCRIPTION: The memvfs.c file implements a custom virtual file system (VFS) to store an entire SQLite database in RAM, providing sample code for creating a simple VFS setup.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/misc/README.md#2025-04-22_snippet_4

LANGUAGE: C
CODE:
```
// Custom in-memory VFS
// Stores SQLite db in RAM
#include <sqlite3.h>
// Additional C code implementation...
```

----------------------------------------

TITLE: Building SQLite Command-Line Executable using nmake
DESCRIPTION: Compiles the SQLite command-line interface (`sqlite3.exe`) using `nmake` and `Makefile.msc`. This target builds the interactive shell for managing SQLite databases and does not require TCL.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-windows.md#2025-04-22_snippet_4

LANGUAGE: cmd
CODE:
```
nmake /f makefile.msc sqlite3.exe
```

----------------------------------------

TITLE: Recursive Table Creation with JSON Data in SQLite
DESCRIPTION: Creates a temporary table 't2' to store JSON text using recursive common table expressions. It generates JSON objects with random values and inserts them into 't2', demonstrating complex JSON operations and recursive data generation.
SOURCE: https://github.com/sqlite/sqlite/blob/master/test/json/jsonb-q1.txt#2025-04-22_snippet_2

LANGUAGE: SQL
CODE:
```
CREATE TEMP TABLE t2(x JSON TEXT);
WITH RECURSIVE
  c(x) AS (VALUES(1) UNION ALL SELECT x+1 FROM c WHERE x<25000),
  array1(y) AS (
    SELECT json_group_array(
             json_object('x',x,'y',random(),'z',hex(randomblob(50)))
           )
      FROM c
  ),
  c2(n) AS (VALUES(1) UNION ALL SELECT n+1 FROM c2 WHERE n<5)
INSERT INTO t2(x)
  SELECT jsonb_object('a',n,'b',n*2,'c',y,'d',3,'e',5,'f',6) FROM array1, c2;
```

----------------------------------------

TITLE: Indexing and Updating JSON Fields in SQLite
DESCRIPTION: Creates indexes on specific JSON attributes within the 't2' table and performs updates using JSON functions. This includes incrementing and decrementing JSON values and removing a JSON field, showcasing SQLite's JSON manipulation capabilities.
SOURCE: https://github.com/sqlite/sqlite/blob/master/test/json/jsonb-q1.txt#2025-04-22_snippet_3

LANGUAGE: SQL
CODE:
```
CREATE INDEX t2x1 ON t2(x->>'a');
CREATE INDEX t2x2 ON t2(x->>'b');
CREATE INDEX t2x3 ON t2(x->>'e');
CREATE INDEX t2x4 ON t2(x->>'f');
UPDATE t2 SET x=jsonb_replace(x,'$.f',(x->>'f')+1);
UPDATE t2 SET x=jsonb_set(x,'$.e',(x->>'f')-1);
UPDATE t2 SET x=jsonb_remove(x,'$.d');
```

----------------------------------------

TITLE: Using the Java JNI Binding to Register a Custom SQLite Collation
DESCRIPTION: Provides an example of registering a custom collation named 'mycollation' using the idiomatic Java JNI binding. It utilizes an anonymous inner class implementing `SomeCallbackType` to define the required `call` method (for comparison), an optional `xDestroy` method, and include optional local state within the callback object itself.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/jni/README.md#2025-04-22_snippet_5

LANGUAGE: java
CODE:
```
int rc = sqlite3_create_collation(db, "mycollation", SQLITE_UTF8, new SomeCallbackType(){

  // Required comparison function:
  @Override public int call(byte[] lhs, byte[] rhs){ ... }

  // Optional finalizer function:
  @Override public void xDestroy(){ ... }

  // Optional local state:
  private String localState1 =
    "This is local state. There are many like it, but this one is mine.";
  private MyStateType localState2 = new MyStateType();
  ...
});
```

----------------------------------------

TITLE: SQLite3 JNI Hello World Example
DESCRIPTION: Demonstrates basic database operations using SQLite3 JNI bindings including opening an in-memory database, error handling, and proper resource cleanup. Shows the importance of explicitly closing database connections.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/jni/README.md#2025-04-22_snippet_0

LANGUAGE: java
CODE:
```
import org.sqlite.jni.*;
import static org.sqlite.jni.CApi.*;

...

final sqlite3 db = sqlite3_open(":memory:");
try {
  final int rc = sqlite3_errcode(db);
  if( 0 != rc ){
    if( null != db ){
      System.out.print("Error opening db: "+sqlite3_errmsg(db));
    }else{
      System.out.print("Error opening db: rc="+rc);
    }
    ... handle error ...
  }
  // ... else use the db ...
}finally{
  // ALWAYS close databases using sqlite3_close() or sqlite3_close_v2()
  // when done with them. All of their active statement handles must
  // first have been passed to sqlite3_finalize().
  sqlite3_close_v2(db);
}
```

----------------------------------------

TITLE: Loading ICU Collation Sequences in SQLite
DESCRIPTION: SQL commands demonstrating how to register and use ICU collation sequences with different locales.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/icu/README.txt#2025-04-22_snippet_2

LANGUAGE: sql
CODE:
```
SELECT icu_load_collation('tr_TR', 'turkish');
SELECT icu_load_collation('en_AU', 'australian');

CREATE TABLE aust_turkish_penpals(
  australian_penpal_name TEXT COLLATE australian,
  turkish_penpal_name    TEXT COLLATE turkish
);
```

----------------------------------------

TITLE: Building Statically Linked sqlite3_analyzer.exe using nmake
DESCRIPTION: Builds the `sqlite3_analyzer.exe` utility, statically linking the TCL library into the executable. This requires the static TCL library (e.g., `tcl90s.lib`) to have been built previously and placed in the appropriate TCL `lib` directory. The `STATICALLY_LINK_TCL=1` argument instructs `nmake` to perform the static linking, removing the runtime dependency on the TCL DLL.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-windows.md#2025-04-22_snippet_16

LANGUAGE: cmd
CODE:
```
STATICALLY_LINK_TCL=1
```

LANGUAGE: cmd
CODE:
```
nmake /f Makefile.msc STATICALLY_LINK_TCL=1 sqlite3_analyzer.exe
```

----------------------------------------

TITLE: Creating and Populating Geopoly Virtual Table
DESCRIPTION: Creates a virtual table using geopoly extension and transforms coordinate data into polygon shapes.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/rtree/visual01.txt#2025-04-22_snippet_3

LANGUAGE: sql
CODE:
```
CREATE VIRTUAL TABLE geo1 USING geopoly(type,clr);
INSERT INTO geo1(_shape,type,clr)
  SELECT geopoly_xform(jshape,A,B,-B,A,xoff,yoff), basis.name, xform.clr
    FROM basis, xform, xyoff
   WHERE xyoff.id1=basis.rowid AND xyoff.id2=xform.rowid;
```

----------------------------------------

TITLE: Building SQLDiff Utility using nmake
DESCRIPTION: Builds the `sqldiff.exe` utility program using `nmake` and `Makefile.msc`. This tool computes the differences between two SQLite database files. TCL installation is not required for this build target.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-windows.md#2025-04-22_snippet_5

LANGUAGE: cmd
CODE:
```
nmake /f makefile.msc sqldiff.exe
```

----------------------------------------

TITLE: Complete Example of Using Inline Hex Database in SQLite CLI
DESCRIPTION: Shows a comprehensive example of embedding a hex database dump directly in a script. The example creates a small database with a table and runs queries against it. This allows creating test databases with specific content or deliberate corruptions.
SOURCE: https://github.com/sqlite/sqlite/blob/master/tool/dbtotxt.md#2025-04-22_snippet_1

LANGUAGE: sql
CODE:
```
.open --hexdb
| size 8192 pagesize 4096 filename x9.db
| page 1 offset 0
|      0: 53 51 4c 69 74 65 20 66 6f 72 6d 61 74 20 33 00   SQLite format 3.
|     16: 10 00 01 01 00 40 20 20 00 00 00 04 00 00 00 02   .....@  ........
|     32: 00 00 00 00 00 00 00 00 00 00 00 01 00 00 00 04   ................
|     48: 00 00 00 00 00 00 00 00 00 00 00 01 00 00 00 00   ................
|     80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 04   ................
|     96: 00 2e 30 38 0d 00 00 00 01 0f c0 00 0f c0 00 00   ..08............
|   4032: 3e 01 06 17 11 11 01 69 74 61 62 6c 65 74 31 74   >......itablet1t
|   4048: 31 02 43 52 45 41 54 45 20 54 41 42 4c 45 20 74   1.CREATE TABLE t
|   4064: 31 28 78 2c 79 20 44 45 46 41 55 4c 54 20 78 27   1(x,y DEFAULT x'
|   4080: 66 66 27 2c 7a 20 44 45 46 41 55 4c 54 20 30 29   ff',z DEFAULT 0)
| page 2 offset 4096
|      0: 0d 08 14 00 04 00 10 00 0e 05 0a 0f 04 15 00 10   ................
|     16: 88 02 03 05 90 04 0e 08 00 00 00 00 00 00 00 00   ................
|   1040: 00 00 00 00 ff 87 7c 02 05 8f 78 0e 08 00 00 00   ......|...x.....
|   2064: 00 00 00 ff 0c 0a 01 fb 00 00 00 00 00 00 00 00   ................
|   2560: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 83   ................
|   2576: 78 01 05 87 70 0e 08 00 00 00 00 00 00 00 00 00   x...p...........
|   3072: 00 00 00 00 00 00 00 00 00 ff 00 00 01 fb 00 00   ................
|   3584: 00 00 00 00 00 83 78 00 05 87 70 0e 08 00 00 00   ......x...p.....
|   4080: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 ff   ................
| end x9.db
SELECT rowid FROM t1;
PRAGMA integrity_check;
```

----------------------------------------

TITLE: Building SQLite Amalgamation Source File using nmake
DESCRIPTION: Uses `nmake` with the `Makefile.msc` to generate the `sqlite3.c` amalgamation source file. This single C file contains the entire SQLite library source code, simplifying integration into other projects. No TCL installation is required for this target.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-windows.md#2025-04-22_snippet_3

LANGUAGE: cmd
CODE:
```
nmake /f makefile.msc sqlite3.c
```

----------------------------------------

TITLE: SQL Rot13 Function Template in C for SQLite
DESCRIPTION: The rot13.c file provides a simple SQL function using a rot13 substitution, serving as a template for creating custom SQL functions in SQLite.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/misc/README.md#2025-04-22_snippet_5

LANGUAGE: C
CODE:
```
// Simple rot13 SQL function
// Template for custom functions
#include <sqlite3ext.h>
// Additional C code implementation...
```

----------------------------------------

TITLE: Using ICU Case Mapping Functions in SQLite
DESCRIPTION: Examples of using the ICU-based upper() and lower() functions for Unicode case mapping, including language-specific case mapping with locales.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/icu/README.txt#2025-04-22_snippet_1

LANGUAGE: sql
CODE:
```
upper('abc') -> 'ABC'
lower('ABC') -> 'abc'
lower('I', 'en_us') -> 'i'
lower('I', 'tr_tr') -> 'ı'
```

----------------------------------------

TITLE: Configuring TCL Installation in Unix Environment
DESCRIPTION: Commands for configuring and installing TCL (Tool Command Language) in a user-specific directory. This is an optional step for certain SQLite build targets.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/compile-for-unix.md#2025-04-22_snippet_0

LANGUAGE: bash
CODE:
```
mkdir $HOME/local
```

LANGUAGE: bash
CODE:
```
./configure --prefix=$HOME/local
```

LANGUAGE: bash
CODE:
```
make install
```

----------------------------------------

TITLE: SQL IN Operator Rewrite Example
DESCRIPTION: Shows how SQLite optimizes an IN operator with 1-2 values by rewriting it as OR conditions.
SOURCE: https://github.com/sqlite/sqlite/blob/master/src/in-operator.md#2025-04-22_snippet_1

LANGUAGE: sql
CODE:
```
x IN (y1,y2)
x=y1 OR x=y2
```

----------------------------------------

TITLE: Running Pattern-Matched Tests
DESCRIPTION: Commands to run tests matching a specific pattern like 'fts5'.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/testrunner.md#2025-04-22_snippet_4

LANGUAGE: shell
CODE:
```
./testfixture $TESTDIR/testrunner.tcl fts5%
./testfixture $TESTDIR/testrunner.tcl 'fts5*'
```

----------------------------------------

TITLE: SHA3 SQL Functions in SQLite with Custom Implementation in C
DESCRIPTION: The shathree.c file includes SQL functions sha3() and sha3_query() for computing SHA-3 hashes, avoiding naming conflicts with older extensions by using 'shathree'.
SOURCE: https://github.com/sqlite/sqlite/blob/master/ext/misc/README.md#2025-04-22_snippet_7

LANGUAGE: C
CODE:
```
// SHA-3 SQL functions
// Avoids naming conflicts with SHA-1
#include <sqlite3ext.h>
// Additional C code implementation...
```

----------------------------------------

TITLE: Performing a Dry Run with testrunner.tcl (Shell)
DESCRIPTION: Shows how to use the `--dryrun` option with `testrunner.tcl` for the 'mdevtest' configuration. This logs the shell commands that would be executed to `testrunner.log` instead of running them. Requires `tclsh` and the `testrunner.tcl` script.
SOURCE: https://github.com/sqlite/sqlite/blob/master/doc/testrunner.md#2025-04-22_snippet_8

LANGUAGE: shell
CODE:
```
# Log the shell commmands that make up the mdevtest test.\ntclsh $TESTDIR/testrunner.tcl --dryrun mdevtest"
```

----------------------------------------

TITLE: Parametrizing SQL SELECT Query on JSON in SQLite
DESCRIPTION: Runs a SELECT query on a table 'data1' to retrieve the 'rowid' and a specific label from a JSON field where the label is not null. The label value is set via a parameter, allowing dynamic query execution based on input.
SOURCE: https://github.com/sqlite/sqlite/blob/master/test/json/jsonb-q1.txt#2025-04-22_snippet_1

LANGUAGE: SQL
CODE:
```
.param set $label 'q87'
SELECT rowid, x->>$label FROM data1 WHERE x->>$label IS NOT NULL;
