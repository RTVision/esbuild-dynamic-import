Esbuild Dynamic Import
=============


[![Version](https://img.shields.io/npm/v/mocha-esbuild.svg)](https://npmjs.org/package/@rtvision/esbuild-dynamic-import)
[![Downloads/week](https://img.shields.io/npm/dw/mocha-esbuild.svg)](https://npmjs.org/package/@rtvision/esbuilt-dynamic-import)
[![License](https://img.shields.io/npm/l/mocha-esbuild.svg)](https://github.com/RtVision/esbuild-dynamic-import/blob/master/package.json)

### Features
-Transforms dynamic imports that contain a template string variable into static imports to be processed by esbuild

-Rewrites dynamic imports of js files that contain a template string variable to use absolute path instead
                                                                                       
## Install
```sh
npm i @rtvision/esbuild-dynamic-import
```

I know this was working using the version 0.13.14 of esbuild. It may work for earlier versions depending on when the plugin system was implemented

## Example Usage

```js
import DynamicImport from '@rtvision/esbuild-dynamic-import';
// note depending on your setup you may need to do DynamicImport.default() instead
DynamicImport({ transformExtensions: ['.vue'], changeRelativeToAbsolute: true, filter: /src\/.*\.js$/ }),
```

## Parameters

  ### transformExtensions
   Transforms all dynamic imports that contain a template varable and the imported file extension
   is included in transformExtensions. I.E. `import(``../../${file}.vue``)`
   All imports found will be turned into static imports of every possible valid import it could be.
   It then uses the static imports instead of node dynamically importing the file at runtime

   My use case for this was I wanted esbuild to process the possible imports that are
   .vue (SFC vue) files so they can be processed by the EsbuildVue plugin and made into
   valid javascript that nodejs can run.
  
  ### changeRelativeToAbsolute
   If there exists a dynamic import like `import(``../../$file}.js``)`
   then that in theory could be resolved at runtime just fine by nodejs. The main
   issue is that the relative file path is now different due to the bundled file produced by
   esbuild being in a likely different file location. changeRelativeToAbsolute will fix this issue
   by changing all relative dynamic imports that contain a template literal variable to absolute ones.

   For my use case dynamic imports while using vite are needed to be relative for production builds
   especially since Rollup is currently used and Rollup requires all dynamic imports to be relative.
  
  ### Filter
   The Filter parameter is used to reduce the scope of the file search.

   I set it to just our local source directory, but by default it will search through every
   js file that is not marked as external

## License
MIT © RtVision
