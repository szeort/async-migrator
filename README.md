# async-migrator

Use it to migrate a JS project source code to async-await model by replacing sync function calls to await statements.
The list of functions that must be replaced is of course configurable. 

Example source code:

```
var conn = $.hdb.getConnection();
var result = conn.execute('select * from MY_TABLE');
```

Will be migrated to:

```
var conn = await $.hdb.getConnection();
var result = await conn.execute('select * from MY_TABLE');
```


# Use cases
One typical use case is for example an sync program using fibers/fibrous. Since fibers/fibrous are not working anymore
in node 16+, that implies a big problem to JS projects willing to run on node 16+. The fiber/fibrous is not supported anymore 
for a very good reason - the well working async-await programming model in Node.js. 
The async-await programming model is giving the development experience of a sync programs - short, logical workflow, no callbacks
but without complex workarounds like fiber/fibers and without killing performace blocking Sync functions. 
Another use case would be to replace a blocking sync-function(which is blocking in C-layer) with aync-await function. 
One example is fs.readFile, fs.readFileSync and fs.promises.readFile. The first one is with callback, the second one is
blocking but conveniet, the third one is non-blocking and convenient i.e. based on async-await programming model. 

# Usage

Command line:
node src/index.js <source directory> <target directory> --map <function map JSON>
The function map JSON is a simple string->string map which maps sync function name to async function name. 
There is no "default" function map, since that is very project specific.


# Samples 

Each sample migration shall have a specific functionmap.json

