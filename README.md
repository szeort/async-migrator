# async-migrator

Use it to migrate a JavaScript project source code to **async-await** model by replacing _sync_ function calls to _await_ statements.
The list of functions that must be replaced is configurable. 

Example source code:

```
var conn = $.hdb.getConnection();
var result = conn.execute('select * from MY_TABLE');
```

It will be migrated to:

```
var conn = await $.hdb.getConnection();
var result = await conn.execute('select * from MY_TABLE');
```


# Use cases
A typical use case, for example, is a sync program using _fibers_ or _fibrous_. Since they are not working in Node 16+, this implies a big problem to JS projects willing to run on Node 16+. There's a good logical reason for these packages to not be supported by Node.js anymore: the **async-await** programming model is implemented in Node 16+ and works like a charm.

The **async-await** programming model gives the development experience of a sync programs - short, logical workflow, no callbacks, without complex workarounds like using _fibers_/_fibrous_, and without killing performace with blocking sync functions. 

Another use case could be to replace a blocking sync function (which is blocking in C-layer) with an async-await function. For example:
* *fs.readFile* - this function has a callback
* *fs.readFileSync* - this function is convenient but blocking 
* *fs.promises.readFile* - this function is both convenient and non-blocking, i.e. based on the **async-await** programming model 

# How to use the migrator

In the command line, execute:

```
node src/index.js <source directory> <target directory> --map <function map JSON>
```

The function map JSON is a simple string (string map), which maps a sync function name to an async function name. 
There is no "default" function map, since that is very project specific.


# Samples 

Each sample migration should have a specific **functionmap.json** file.

