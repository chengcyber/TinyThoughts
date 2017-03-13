# Installation

```
npm install -g typescript

tsc -v
```

### Basic Usage

```
tsc app.ts

tsc --out bundle.js app.ts

tsc -w/--watch --out bundle.js app.ts

```

## New TypeScript Project

```
tsc --init
```

tsconfig.json created

```
tsc
```

# Files and outDir in tsconfig.json

```
// tsconfig.json
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "es5",
        "noImplicitAny": false,
        "sourceMap": false,
        "outDir": "./dist"
    },
    "files": [
        "main.ts"
    ]
}

```

# Stop builing TypeScript when Error


```
// tsconfig.json
{
    "compilerOptions": {
        ...
        "noEmitOnError": true
    }
}
```

now, run `tsc`, tsc will not output files when error

# SystemJS as module loader

leverage system.js to use compiled files in browser

npmcdn.com/systemjs

add /

goto dist folder and copy system.js link

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="https://unpkg.com/systemjs@0.20.9/dist/system.js"></script>
</head>
<body>

  <script>
    System.config({
      packages: {
        "dist": {
          "defaultExtensions": "js",
          "main": "main"
        }
      }
    })

    System.import("dist");
  </script>
  
</body>
</html>
```

```
tsc -w

http-server -c-1 

// visit localhost:8080 now

```

# Exclude files and use RooDir instead of Files

Since blob pattern is not supported now (around 2.0, not solved)
new files should be added to Files manually, so use rootDir and exclude instead

```
// tsconfig.json
{
    "compilerOptions": {
        "rootDir": "./src",
        ...
    },
    "exclude": [
        "node_modules"
    ]
}
```

# Target es5 and es6

when import other js module with `import from`, we will get errors, since tsc now working on es5 mode.

```
// tsconfig.json
{
    "compilerOptions": {
        ...
        "target": "es6",
    }
}

```

# 3rd party module type

using typings module to manager 3rd party module types

```
npm install -g typings

typings install lodash --save

typings search react
```

# es6-shim

alternative to solve Can not find Promise, since Promise is es6 feature

```
typings install es6-shim --save --ambient
```

# Emit Decoration Meta Data

```
// tsconfig.json
{
    "compilerOptions": {
        ...
        "emitDecoratorMetadata": true
    }
    ...
}
```

# Genrating Definition Files

```
// tsconfig.json
{
    "compilerOptions": {
        ...
        "declaration": true
    }
}
```

```
// create index.ts to export all stuff in your code
export * from './main'
export * from './social-network'
```

```
// package.json
{
    "typings": "./dist/index.d.ts"
}
```






