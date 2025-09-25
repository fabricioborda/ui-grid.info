# UI-Grid Documentation Hub

Welcome to the comprehensive documentation repository for UI-Grid, a powerful data grid for AngularJS applications.

## 📚 Documentation

Explore our complete technical documentation:

### [📖 Complete Documentation](./docs/README.md)

### Quick Navigation

- **[🚀 Getting Started](./docs/getting-started.md)** - New to UI-Grid? Start here!
- **[⚡ Installation Guide](./docs/installation.md)** - Setup and installation options
- **[🔧 Basic Usage](./docs/basic-usage.md)** - Fundamental concepts and patterns
- **[📋 API Reference](./docs/api-reference.md)** - Complete API documentation
- **[⚙️ Advanced Configuration](./docs/advanced-configuration.md)** - Complex scenarios and customization
- **[💡 Examples & Tutorials](./docs/examples/README.md)** - Working code examples
- **[👨‍💻 Developer Guide](./docs/developer-guide.md)** - Extending and customizing UI-Grid
- **[🤝 Contributing](./docs/contributing.md)** - How to contribute to the project
- **[🔍 Troubleshooting](./docs/troubleshooting.md)** - Common issues and solutions
- **[❓ FAQ](./docs/faq.md)** - Frequently asked questions

## 🌟 About UI-Grid

UI-Grid is a native AngularJS grid that provides a robust, feature-rich data grid component for web applications. Key features include:

- **Virtualization** - Handle large datasets efficiently
- **Sorting & Filtering** - Multi-column sorting and flexible filtering
- **Inline Editing** - Edit data directly in the grid
- **Selection** - Row and cell selection with multi-select support
- **Pagination** - Client-side and server-side pagination
- **Column Features** - Resizing, pinning, grouping, and reordering
- **Export** - Export data to CSV, PDF, and Excel formats
- **Accessibility** - ARIA compliance and keyboard navigation

## 🔗 Quick Links

- **Live Demo**: [http://ui-grid.info](http://ui-grid.info)
- **Source Code**: [https://github.com/angular-ui/ng-grid](https://github.com/angular-ui/ng-grid)
- **Issue Tracker**: [https://github.com/angular-ui/ng-grid/issues](https://github.com/angular-ui/ng-grid/issues)

## 🚀 Quick Start

```html
<!-- Include UI-Grid CSS and JS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.css">
<script src="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.js"></script>

<!-- Add to your Angular app -->
<div ng-controller="MyController">
  <div ui-grid="gridOptions" class="grid"></div>
</div>
```

```javascript
// Controller
angular.module('myApp', ['ui.grid']).controller('MyController', function($scope) {
  $scope.gridOptions = {
    data: [
      { name: 'John', age: 25, city: 'New York' },
      { name: 'Jane', age: 30, city: 'London' },
      { name: 'Bob', age: 35, city: 'Paris' }
    ]
  };
});
```

```css
.grid {
  width: 100%;
  height: 400px;
}
```

## 💻 Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Internet Explorer 9+

## 📄 License

UI-Grid is released under the [MIT License](./LICENSE).

---

**Note**: The live website at [ui-grid.info](http://ui-grid.info) is generated from the main UI-Grid repository during the build process. The actual source code is located at [https://github.com/angular-ui/ng-grid](https://github.com/angular-ui/ng-grid). Please raise any issues or contributions there.
