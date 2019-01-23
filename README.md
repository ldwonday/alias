# gulp-ts-alias

Resolve TypeScript import aliases and paths defined in `tsconfig`.

## Install

`npm install --save-dev gulp-ts-alias`

## Usage

```javascript
const typescript = require('gulp-typescript');
const sourcemaps = require('gulp-sourcemaps');
const alias = require('gulp-ts-alias');

const project = typescript.createProject('tsconfig.json');

function build() {
  const compiled = src('./src/**/*.ts')
    .pipe(alias({ configuration: project.config }))
    .pipe(sourcemaps.init())
    .pipe(project());

  return compiled.js
    .pipe(sourcemaps.write({ sourceRoot: file => path.relative(path.join(file.cwd, file.path), file.base) }))
    .pipe(dest('build/'))
}
```

## Example

The following configuration is common in `tsconfig` configuration files

```json
{
  "rootDir": "./src",
  "baseUrl": ".",
  "paths": {
    "@/*": ["src/*"],
  }
}
```

In practice, these path aliases are often used in this fashion

Input:

```typescript
import express from 'express';

import A from './file'; // Normal relative import

// Aliased import, resolves to some relative path to /src/components
import B from '@/components';
```

Output:

```typescript
import express from 'express';

import A from './file';

// gulp-ts-alias finds the correct relative path
import B from '../../components';
```
