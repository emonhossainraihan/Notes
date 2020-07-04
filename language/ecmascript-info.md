every time you use a `require()`, you’re in fact using the implementation of **CommonJS** ES modules — or just **ESM**, which comes within Node.js by default.

Remember, ESM can’t be imported via the require function. On the other side, if you try the import from syntax, we’ll get an error because CommonJS files are not allowed to use it

The `@std/esm` rewrites require and also **adds functionality** to the Node version module being used. It does some inline and on-demand transformations, processing and caching to the executions in real time.

When dealing with codebase conversions, remember these **important points**:

- When migrating your **js** files to **mjs**, change the basic exports (**module.exports**) to the new ESM **export** statement
- All the **requires** must be changed to the respective **import** statements
- If you’re using require dynamically, remember to make the import as well, via **await import** (or the dynamic import() function we’ve seen)
- Also change the other requires in other files that reference what you’re migrating
- **mjs** files, when used in the browser, must be served with the correct Media Type, which is text/javascript or application/javascript. Since browsers don’t care about the extension, Node.js is the only thing that requires the extension to exist. This is the way it can detect whether a file is a **CJS** or an **ES module**


## [ES modules: A cartoon deep-dive](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)

ES Modules is the ECMAScript standard for working with modules. While Node.js has been using the CommonJS standard since years, the browser never had a module system, as every major decision such as a module system must be first standardized by ECMAScript and then implemented.
</br>
ES Modules Syntax: `import package from 'module-name'` </br>
CommonJS uses: `const package = require('module-name')
`

The [ES module spec](https://tc39.es/ecma262/#sec-modules) says how you should parse files into module records, and how you should instantiate and evaluate that module. However, it doesn’t say how to get the files in the first place.

It’s the loader that fetches the files. And the loader is specified in a different specification. For browsers, that spec is the [HTML spec](https://html.spec.whatwg.org/#fetch-a-module-script-tree). But you can have different loaders based on what platform you are using.

![](../images/es-module1.png)

The loader also controls exactly how the modules are loaded. It calls the ES module methods — `ParseModule`, `Module.Instantiate`, and `Module.Evaluate`. It’s kind of like a puppeteer controlling the JS engine’s strings.

![](../images/es-module2.png)

## Construction

Three things happen for each module during the Construction phase.

- Figure out where to download the file containing the module from (aka module resolution)
- Fetch the file (by downloading it from a URL or loading it from the file system)
- Parse the file into a module record

![](../images/es-module3.png)

## Parsing
Now that we have fetched this file, we need to parse it into a module record. Once the module record is created, it is placed in the module map. In browsers you just put `type="module"` on the script tag. This tells the browser that this file should be parsed as a module. And since only modules can be imported, the browser knows that any imports are modules, too.

But in Node, you don’t use HTML tags, so you don’t have the option of using a type attribute. One way the community has tried to solve this is by using an .mjs extension. Using that extension tells Node, “this file is a module”. You’ll see people talking about this as the signal for the parse goal. The discussion is currently ongoing, so it’s unclear what signal the Node community will decide to use in the end.

Either way, the loader will determine whether to parse the file as a module or not. If it is a module and there are imports, it will then start the process over again until all of the files are fetched and parsed.

And we’re done! At the end of the loading process, you’ve gone from having just an entry point file to having a bunch of module records.

## Instantiation

Like I mentioned before, an instance combines code with state. That state lives in memory, so the instantiation step is all about wiring things up to memory.

First, the JS engine creates a module environment record. This manages the variables for the module record. Then it finds boxes in memory for all of the exports. The module environment record will keep track of which box in memory is associated with each export.

The engine finishes wiring up all of the exports below a module — all of the exports that the module depends on. Then it comes back up a level to wire up the imports from that module.

Note that both the export and the import point to the same location in memory. Wiring up the exports first guarantees that all of the imports can be connected to matching exports.

This is different from CommonJS modules. In CommonJS, the entire export object is copied on export. This means that any values (like numbers) that are exported are copies.

This means that if the exporting module changes that value later, the importing module doesn’t see that change.

## Evaluation

The final step is filling in these boxes in memory. The JS engine does this by executing the top-level code — the code that is outside of functions.
