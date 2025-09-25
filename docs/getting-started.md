# Getting Started with UI-Grid

UI-Grid is a powerful, feature-rich data grid for AngularJS applications. This guide will help you get up and running quickly.

## Prerequisites

Before you begin, ensure you have:

- AngularJS 1.2.x or higher
- A modern web browser
- Basic knowledge of AngularJS concepts

## System Requirements

- **AngularJS**: Version 1.2.x - 1.8.x
- **Browser**: Modern browsers (Chrome, Firefox, Safari, Edge, IE9+)
- **Dependencies**: None (UI-Grid is self-contained)

## Quick Start

### 1. Install UI-Grid

Choose one of the following methods:

**Via CDN:**
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.css">
<script src="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.js"></script>
```

**Via npm:**
```bash
npm install angular-ui-grid
```

**Via Bower:**
```bash
bower install angular-ui-grid
```

### 2. Include in Your App

Add the UI-Grid module to your application:

```javascript
angular.module('myApp', ['ui.grid']);
```

### 3. Create Your First Grid

**HTML Template:**
```html
<div ng-controller="MainCtrl">
  <div ui-grid="gridOptions" class="grid"></div>
</div>
```

**Controller:**
```javascript
angular.module('myApp').controller('MainCtrl', ['$scope', function ($scope) {
  $scope.gridOptions = {
    data: [
      { name: 'John', age: 25, city: 'New York' },
      { name: 'Jane', age: 30, city: 'London' },
      { name: 'Bob', age: 35, city: 'Paris' }
    ]
  };
}]);
```

**CSS:**
```css
.grid {
  width: 100%;
  height: 400px;
}
```

## What's Next?

- [Basic Usage](./basic-usage.md) - Learn the fundamentals
- [Installation Guide](./installation.md) - Detailed installation options
- [Examples](./examples/README.md) - See UI-Grid in action
- [API Reference](./api-reference.md) - Complete API documentation

## Common First Steps

1. **Column Configuration**: Define column properties, widths, and types
2. **Data Binding**: Connect your data source to the grid
3. **Styling**: Customize the grid appearance
4. **Features**: Enable sorting, filtering, editing, and more

## Need Help?

- Check the [FAQ](./faq.md) for common questions
- Visit the [Troubleshooting](./troubleshooting.md) guide
- Browse [Examples](./examples/README.md) for code samples
- Open an issue on [GitHub](https://github.com/angular-ui/ng-grid/issues)