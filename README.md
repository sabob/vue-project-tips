# vue-project-tips
Tips for a Vue project

Create your Vue project with vue-cli.

If you want to separate your templates from your JS files, you need to precompile the templates.

JS Modules doesn't support import HTML files, so this won't work:

```
import template from '.template.html';
```

When the HTML file is imported it will be interpreted as a JS file and break.

The solution is the precompile the template into a Vue render function and so make he bundler happy. Precompiling templates also has a performance benefit in that the template doesn't have to be compiled during runtime and the Vue compiler doesn't have to be bundled with the project.

To precompile the templates add the following dependency to *package.json*:

```
"vue-template-compiler-loader": "^1.0.4"
```

now update your project:
```javascript
prompt>npm i
```

Then add this snippet to your *vue.config.js* in the root of your project (if vue.config.js doesn't exist, create it in the roof of your project):

```javascript
const path = require('path')

module.exports = {

    runtimeCompiler: true,

    baseUrl: '/test',

    configureWebpack: {
        module: {
            rules: [
                {
                    test: /\.html$/,
                    use: 'vue-template-compiler-loader',
                    include: [
                        path.resolve(__dirname, 'src')
                    ]
                }
            ]
        }
    }
}
```

This says that all *.html files must be loaded with the loader *vue-template-compiler-loader* as specified in *package.json*.

Typescript
----------
If your project uses typescript, your IDE might show errors when importing an html template because it won't know the type of the module.

To let Typescript know how to interpret html imports we create a *shims* file called *shims.html.d.ts* in the root of your project:

```javascript
declare module '*.html' {
    var _: any;
    export default  _;
}
```

Here we declare a module to handle *\*.html* files as type *any*.
