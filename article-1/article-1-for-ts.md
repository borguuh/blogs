# Module Path Aliases With Typescript

This article is a follow up to my previous article here on the same topic, albeit with typescript this time around. You may want to read more there to understand the "why" behind the article.

## Creating module aliases with tsconfig.json
There are three config properties to be aware of to achieve this. These are the compilerOptions, baseUrl and the paths properties.

It is as simple as creating a json object in it with compilerOptions key. Then specifying the project base url with the "baseUrl" option, then the various paths to each file you want to reference with the paths option while using the reference key value pair, an example is as shown below.

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["src/components/*"],
      "@styles/*": ["src/styles/*"],
      "@config/*": ["src/config/*"],
      "@context/*": ["src/context/*"],
      "@helpers/*": ["src/helpers/*"]
    }
  }
}
```

In the example above, there five (5) folders named components, context, config, styles and helpers. importing from the "@/components" will automatically go to the /components folder wherever you are in the file tree.

## Calling reference paths by their alias

To import any file from a referenced folder, for example, to import a component named Layout from the component folder, we replace "../../../components/Layout" with "@components/Layout". Wow! Easy, right? Yeah. It reduces the probability of making errors during imports, especially if a project has more than one file with the same name. All you have to know is that one is in the component folder and the other is in whatever folder.

Below is a snippet that uses three (3) of the folders referenced above

```
import Layout from '@components/Layout'
import { API_URL } from '@config/index'
import styles from '@styles/Form.module.css'
```

## Avoid Cannot find module `'@components/Layout'` error after compile

To avoid `Error: Cannot find module '@components/Layout'` after code is compiled to javascript. Install the `module-alias` package with npm or your favorite package installer.

`npm i --save module-alias`

Then register and add the custom module path aliases (aliases of directories) to the package.json file as done below. 
NB: dist folder houses the compiled javascript files.

```
"_moduleAliases": {
      "@components": "dist/src/components",
      "@config": "dist/src/config",
      "@context": "dist/src/context",
      "@helpers": "dist/src/helpers"
}
```
Finally, add this line at the top of the main file (entry point) of your app e.g the index.js in react for example.

`import 'module-alias/register';`

You can now use module aliases in your import / require satements henceforth.
