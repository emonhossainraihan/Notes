```json
// jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
        "@components/": ["components/"], 
        "@styles/": ["styles/*"],
        "@libs/": ["libs/*"]
      }
    }
 }
```

Instead of using `../../../styles/` use `@styles/`.
