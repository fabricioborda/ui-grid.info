# Installation Guide

This guide covers all the different ways to install and set up UI-Grid in your AngularJS application.

## Installation Methods

### 1. CDN (Recommended for Quick Start)

The fastest way to get started is by using a CDN:

```html
<!-- CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.css">

<!-- JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.js"></script>
```

Alternative CDNs:
```html
<!-- UNPKG -->
<link rel="stylesheet" href="https://unpkg.com/angular-ui-grid@4.11.1/ui-grid.min.css">
<script src="https://unpkg.com/angular-ui-grid@4.11.1/ui-grid.min.js"></script>

<!-- CDNJS -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-grid/4.11.1/ui-grid.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-grid/4.11.1/ui-grid.min.js"></script>
```

### 2. npm

For modern build systems and Node.js projects:

```bash
npm install angular-ui-grid --save
```

Then import in your application:

```javascript
// ES6 modules
import 'angular-ui-grid/ui-grid.css';
import 'angular-ui-grid';

// CommonJS
require('angular-ui-grid/ui-grid.css');
require('angular-ui-grid');
```

### 3. Bower

For Bower-based projects:

```bash
bower install angular-ui-grid --save
```

Include files:
```html
<link rel="stylesheet" href="bower_components/angular-ui-grid/ui-grid.css">
<script src="bower_components/angular-ui-grid/ui-grid.js"></script>
```

### 4. Download Direct

1. Go to [GitHub releases](https://github.com/angular-ui/ng-grid/releases)
2. Download the latest version
3. Extract and include files:

```html
<link rel="stylesheet" href="path/to/ui-grid.css">
<script src="path/to/ui-grid.js"></script>
```

## Build Systems Integration

### Webpack

```javascript
// webpack.config.js
module.exports = {
  // ... other config
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.(woff|woff2|ttf|eot|svg)$/,
        use: 'file-loader'
      }
    ]
  }
};
```

### Grunt

```javascript
// Gruntfile.js
grunt.initConfig({
  copy: {
    uigrid: {
      files: [
        {
          src: 'node_modules/angular-ui-grid/ui-grid.min.css',
          dest: 'dist/css/ui-grid.min.css'
        },
        {
          src: 'node_modules/angular-ui-grid/ui-grid.min.js',
          dest: 'dist/js/ui-grid.min.js'
        }
      ]
    }
  }
});
```

### Gulp

```javascript
// gulpfile.js
const gulp = require('gulp');

gulp.task('copy-ui-grid', function() {
  return gulp.src([
    'node_modules/angular-ui-grid/ui-grid.min.css',
    'node_modules/angular-ui-grid/ui-grid.min.js'
  ])
  .pipe(gulp.dest('dist/vendor/ui-grid'));
});
```

## Module Registration

After including the files, register the module:

```javascript
// Basic registration
angular.module('myApp', ['ui.grid']);

// With additional UI-Grid modules
angular.module('myApp', [
  'ui.grid',
  'ui.grid.selection',
  'ui.grid.edit',
  'ui.grid.cellNav'
]);
```

## Available Modules

UI-Grid is modular. Include only what you need:

- `ui.grid` - Core grid functionality
- `ui.grid.selection` - Row/cell selection
- `ui.grid.edit` - Inline editing
- `ui.grid.cellNav` - Cell navigation
- `ui.grid.pinning` - Column pinning
- `ui.grid.resizeColumns` - Column resizing
- `ui.grid.moveColumns` - Column reordering
- `ui.grid.grouping` - Row grouping
- `ui.grid.treeView` - Tree view
- `ui.grid.pagination` - Client-side pagination
- `ui.grid.infiniteScroll` - Infinite scrolling
- `ui.grid.exporter` - Export functionality

## Verification

Verify installation by checking in browser console:

```javascript
// Should return the UI-Grid version
console.log(angular.module('ui.grid').version);
```

## Common Issues

### CSS Not Loading
- Ensure CSS is included before JavaScript
- Check file paths are correct
- Verify CORS headers for CDN

### Module Not Found
```
Error: [$injector:modulerr] Failed to instantiate module myApp due to:
Error: [$injector:modulerr] Failed to instantiate module ui.grid
```

**Solution**: Ensure ui-grid.js is loaded before your app module.

### Font Issues
UI-Grid uses font icons. If you see squares instead of icons:
- Include the entire CSS file (contains embedded fonts)
- Check Content Security Policy (CSP) settings
- Ensure proper MIME types for font files

## Next Steps

- [Basic Usage](./basic-usage.md) - Start using UI-Grid
- [Getting Started](./getting-started.md) - Quick start guide
- [Examples](./examples/README.md) - See examples