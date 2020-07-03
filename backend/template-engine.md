**Templating:** Basically the idea is to have some string containing placeholders for values as input, and use some function to replace those placeholders with 
real values.

JavaScript template engines are often used when writing thick JS-clients or as "normal" template engines when using server-side js like Node.js.

Instead of cluttering the code by generating HTML using string concatenating etc., you instead use a template and interpolate the variable in them.

A **template processor** (also known as a **template engine** or **template parser**) is software designed to combine templates with a 
data model to produce **result documents**.
The language that the templates are written in is known as a template language or templating language. For purposes of this article,
a result document is any kind of 
formatted output, including **documents**, **web pages**, or **source code** (in source code generation), either in whole or in fragments. 
A template engine is ordinarily included 
as a part of a web template system or application framework, and may be used also as a preprocessor or filter. 

![](/images/template-engine.png)

Template engines typically include features common to most high-level programming languages, with an emphasis on features for processing plain text.

Such features include:

- variables and functions
- text replacement
- file inclusion (or transclusion)
- conditional evaluation and loops

All template processing systems consist of at least these primary elements:

- an associated data model;
- one or more source templates;
- a processor or template engine;
- generated output in the form of result documents.
