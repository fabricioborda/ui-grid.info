# Frequently Asked Questions (FAQ)

Answers to commonly asked questions about UI-Grid.

## General Questions

### Q: What is UI-Grid?
**A:** UI-Grid is a powerful, feature-rich data grid component for AngularJS applications. It provides virtualization, sorting, filtering, editing, selection, and many other features for displaying and manipulating tabular data.

### Q: Is UI-Grid still maintained?
**A:** UI-Grid is in maintenance mode. While it's stable and widely used, active development has slowed down. For new projects, consider modern alternatives like ag-Grid or other contemporary grid solutions.

### Q: What versions of AngularJS does UI-Grid support?
**A:** UI-Grid supports AngularJS versions 1.2.x through 1.8.x. It does not support Angular 2+ (the modern Angular framework).

### Q: Can I use UI-Grid with Angular (2+)?
**A:** No, UI-Grid is specifically designed for AngularJS (1.x). For Angular 2+, consider alternatives like ag-Grid, ng-grid, or PrimeNG DataTable.

## Installation & Setup

### Q: How do I install UI-Grid?
**A:** You can install UI-Grid via:
- **CDN:** Include CSS and JS files from a CDN
- **npm:** `npm install angular-ui-grid`
- **Bower:** `bower install angular-ui-grid`
- **Direct download:** From GitHub releases

See the [Installation Guide](./installation.md) for detailed instructions.

### Q: What files do I need to include?
**A:** You need both CSS and JavaScript files:
```html
<link rel="stylesheet" href="ui-grid.css">
<script src="ui-grid.js"></script>
```

### Q: Do I need to include AngularJS separately?
**A:** Yes, UI-Grid requires AngularJS but doesn't include it. You must include AngularJS before UI-Grid.

### Q: Why am I getting "Module 'ui.grid' not found" error?
**A:** This usually means:
1. UI-Grid JavaScript file isn't loaded
2. UI-Grid is loaded after your app module
3. File path is incorrect
4. Network/CDN issue

## Configuration

### Q: How do I set up a basic grid?
**A:** Minimum setup requires:
```javascript
$scope.gridOptions = {
  data: myDataArray
};
```
```html
<div ui-grid="gridOptions" class="grid"></div>
```

### Q: How do I define custom columns?
**A:** Use the `columnDefs` property:
```javascript
$scope.gridOptions = {
  columnDefs: [
    { field: 'name', displayName: 'Full Name', width: 200 },
    { field: 'age', type: 'number', width: 100 },
    { field: 'email', width: 250 }
  ],
  data: myData
};
```

### Q: Why doesn't my grid have a height?
**A:** UI-Grid requires an explicit height. Set it via CSS:
```css
.grid {
  height: 400px; /* Required */
}
```

### Q: How do I make the grid responsive?
**A:** Use percentage widths and CSS media queries:
```javascript
columnDefs: [
  { field: 'name', width: '30%' },
  { field: 'description', width: '*' }, // Takes remaining space
  { field: 'date', width: '20%' }
]
```

## Features

### Q: How do I enable sorting?
**A:** Sorting is enabled by default. To control it:
```javascript
$scope.gridOptions = {
  enableSorting: true, // Global setting
  columnDefs: [
    { field: 'name', enableSorting: true },
    { field: 'actions', enableSorting: false }
  ]
};
```

### Q: How do I enable filtering?
**A:** Enable filtering globally:
```javascript
$scope.gridOptions = {
  enableFiltering: true
};
```

### Q: How do I enable row selection?
**A:** Include the selection module and directive:
```javascript
angular.module('myApp', ['ui.grid', 'ui.grid.selection']);
```
```html
<div ui-grid="gridOptions" ui-grid-selection></div>
```
```javascript
$scope.gridOptions = {
  enableRowSelection: true,
  multiSelect: true
};
```

### Q: How do I enable inline editing?
**A:** Include the edit module and directive:
```javascript
angular.module('myApp', ['ui.grid', 'ui.grid.edit']);
```
```html
<div ui-grid="gridOptions" ui-grid-edit></div>
```
```javascript
$scope.gridOptions = {
  columnDefs: [
    { field: 'name', enableCellEdit: true }
  ]
};
```

### Q: How do I add pagination?
**A:** Include the pagination module:
```javascript
angular.module('myApp', ['ui.grid', 'ui.grid.pagination']);
```
```html
<div ui-grid="gridOptions" ui-grid-pagination></div>
```
```javascript
$scope.gridOptions = {
  paginationPageSizes: [25, 50, 75],
  paginationPageSize: 25
};
```

## Data Handling

### Q: How do I load data from a server?
**A:** Use `$http` or your preferred HTTP client:
```javascript
$http.get('/api/data').then(function(response) {
  $scope.gridOptions.data = response.data;
});
```

### Q: How do I update data dynamically?
**A:** Just update the data array:
```javascript
// Add new item
$scope.gridOptions.data.push(newItem);

// Remove item
var index = $scope.gridOptions.data.indexOf(itemToRemove);
$scope.gridOptions.data.splice(index, 1);

// Replace entire dataset
$scope.gridOptions.data = newDataArray;
```

### Q: Why doesn't my data update in the grid?
**A:** Common causes:
1. Data changes outside Angular context (use `$scope.$apply()`)
2. Reference not changed (create new array instead of modifying existing)
3. Async timing issues

### Q: How do I handle large datasets?
**A:** UI-Grid provides several strategies:
1. **Virtualization** (enabled by default for 20+ rows)
2. **Pagination** (client-side or server-side)
3. **Infinite scrolling** with the `ui.grid.infiniteScroll` module

### Q: Can I use nested/complex objects in my data?
**A:** Yes, use dot notation in field names:
```javascript
columnDefs: [
  { field: 'user.name', displayName: 'User Name' },
  { field: 'address.city', displayName: 'City' }
]
```

## Customization

### Q: How do I create custom cell templates?
**A:** Use the `cellTemplate` property:
```javascript
columnDefs: [
  {
    field: 'name',
    cellTemplate: '<div class="ui-grid-cell-contents"><strong>{{row.entity.name}}</strong></div>'
  },
  {
    field: 'actions',
    cellTemplate: '<button ng-click="grid.appScope.edit(row.entity)">Edit</button>'
  }
]
```

### Q: How do I style the grid?
**A:** Use CSS to override default styles:
```css
.my-grid .ui-grid-header {
  background-color: #337ab7;
  color: white;
}

.my-grid .ui-grid-row:nth-child(odd) .ui-grid-cell {
  background-color: #f9f9f9;
}
```

### Q: How do I add custom buttons/actions?
**A:** Use custom cell templates:
```javascript
columnDefs: [
  {
    field: 'actions',
    cellTemplate: `
      <div class="ui-grid-cell-contents">
        <button ng-click="grid.appScope.edit(row.entity)">Edit</button>
        <button ng-click="grid.appScope.delete(row.entity)">Delete</button>
      </div>
    `
  }
]
```

### Q: How do I handle events?
**A:** Use the grid API and event system:
```javascript
$scope.gridOptions = {
  onRegisterApi: function(gridApi) {
    $scope.gridApi = gridApi;
    
    gridApi.selection.on.rowSelectionChanged($scope, function(row) {
      console.log('Row selected:', row.entity);
    });
    
    gridApi.edit.on.afterCellEdit($scope, function(rowEntity, colDef, newValue, oldValue) {
      console.log('Cell edited:', colDef.field, oldValue, '->', newValue);
    });
  }
};
```

## Performance

### Q: My grid is slow with large datasets. What can I do?
**A:** Several optimization strategies:
1. **Virtualization** is enabled automatically for 20+ rows
2. Use **pagination** to limit displayed rows
3. Implement **server-side filtering/sorting**
4. Use `rowIdentity` for better change detection
5. Avoid complex calculations in cell templates

### Q: How does virtualization work?
**A:** UI-Grid only renders visible rows in the DOM. As you scroll, rows are created/destroyed dynamically. This allows handling datasets with thousands of rows efficiently.

### Q: Should I use client-side or server-side pagination?
**A:** 
- **Client-side:** Good for smaller datasets (< 1000 rows)
- **Server-side:** Better for large datasets, reduces memory usage and initial load time

## Troubleshooting

### Q: Why can't I see my grid?
**A:** Common issues:
1. Missing height on grid container
2. Data not loaded yet
3. CSS files not included
4. JavaScript errors preventing rendering

### Q: Why aren't my icons showing?
**A:** UI-Grid uses font icons embedded in the CSS file. Ensure:
1. Complete CSS file is loaded
2. No CSP (Content Security Policy) blocking fonts
3. Proper MIME types configured for font files

### Q: Grid works in Chrome but not IE. Why?
**A:** IE requires additional polyfills for some features:
```html
<script src="https://polyfill.io/v3/polyfill.min.js"></script>
```

### Q: How do I debug grid issues?
**A:** 
1. Check browser console for errors
2. Enable debug mode: `$scope.gridOptions.debug = true`
3. Inspect grid API in console: `window.gridApi = gridApi`
4. Use browser dev tools to inspect DOM structure

## Migration & Alternatives

### Q: Should I use UI-Grid for new projects?
**A:** While UI-Grid is stable, for new projects consider:
- **ag-Grid:** Modern, actively developed, supports Angular/React/Vue
- **Kendo UI Grid:** Commercial option with extensive features
- **PrimeNG DataTable:** For Angular projects
- **React Table:** For React applications

### Q: How do I migrate from UI-Grid?
**A:** Migration strategy depends on your target:
1. **Identify used features** (sorting, filtering, editing, etc.)
2. **Map to new grid's API**
3. **Update templates** and styling
4. **Test thoroughly** with your data scenarios

### Q: Is there a React/Vue version of UI-Grid?
**A:** No, UI-Grid is AngularJS-specific. For other frameworks:
- **React:** React Table, ag-Grid
- **Vue:** Vue Good Table, ag-Grid
- **Angular:** ag-Grid, PrimeNG

## Community & Support

### Q: Where can I get help?
**A:** 
- **GitHub Issues:** For bugs and feature requests
- **Stack Overflow:** Tag questions with `angular-ui-grid`
- **Documentation:** This comprehensive guide
- **Examples:** Working code samples in our examples section

### Q: How do I report a bug?
**A:** 
1. Create a minimal reproduction case
2. Open an issue on [GitHub](https://github.com/angular-ui/ng-grid/issues)
3. Include browser/version information
4. Provide sample code demonstrating the issue

### Q: Can I contribute to UI-Grid?
**A:** Yes! See the [Contributing Guidelines](./contributing.md) for details on:
- Code contributions
- Documentation improvements
- Bug reporting
- Feature requests