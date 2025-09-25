# Troubleshooting Guide

Common issues and solutions when working with UI-Grid.

## Installation Issues

### Module Not Found Error

**Error:**
```
Error: [$injector:modulerr] Failed to instantiate module myApp due to:
Error: [$injector:modulerr] Failed to instantiate module ui.grid
```

**Cause:** UI-Grid JavaScript file not loaded or loaded after your app module.

**Solutions:**
1. Ensure ui-grid.js is loaded before your application:
   ```html
   <script src="angular.js"></script>
   <script src="ui-grid.js"></script>
   <script src="your-app.js"></script>
   ```

2. Check file paths and network requests in browser dev tools

3. Verify CDN links are correct and accessible

### CSS Not Loading

**Symptoms:** Grid appears as plain table, no styling, missing icons

**Solutions:**
1. Include UI-Grid CSS file:
   ```html
   <link rel="stylesheet" href="ui-grid.css">
   ```

2. Check Content Security Policy (CSP) if using inline styles/fonts

3. Verify CSS file path and ensure it's accessible

### Font Icons Not Displaying

**Symptoms:** Square boxes instead of sort/menu icons

**Solutions:**
1. Ensure complete CSS file is loaded (contains embedded fonts)
2. Check browser console for font loading errors
3. Verify MIME types for font files on your server
4. Check CSP settings if using strict policies

## Performance Issues

### Slow Rendering with Large Datasets

**Symptoms:** Grid takes long time to load or becomes unresponsive

**Solutions:**
1. Enable virtualization (enabled by default):
   ```javascript
   $scope.gridOptions = {
     virtualizationThreshold: 20, // Default value
     data: largeDataset
   };
   ```

2. Use server-side pagination:
   ```javascript
   $scope.gridOptions = {
     useExternalPagination: true,
     paginationPageSize: 50,
     totalItems: totalCount
   };
   ```

3. Implement data filtering on the server side

4. Use `rowIdentity` for better change detection:
   ```javascript
   $scope.gridOptions = {
     rowIdentity: function(row) {
       return row.id;
     }
   };
   ```

### Memory Leaks

**Symptoms:** Browser memory usage increases over time

**Solutions:**
1. Properly destroy event listeners:
   ```javascript
   $scope.$on('$destroy', function() {
     if ($scope.gridApi) {
       // Clean up any manual event listeners
     }
   });
   ```

2. Avoid creating new objects in cell templates on every digest cycle

3. Use `track by` in ng-repeat if used in custom templates

## Display Issues

### Grid Not Visible

**Symptoms:** Grid container exists but no content is shown

**Solutions:**
1. Set explicit height:
   ```css
   .grid {
     height: 400px; /* Required */
   }
   ```

2. Check if data is available:
   ```javascript
   console.log('Grid data:', $scope.gridOptions.data);
   ```

3. Call refresh if grid is initially hidden:
   ```javascript
   $timeout(function() {
     $scope.gridApi.core.handleWindowResize();
   });
   ```

### Column Width Issues

**Symptoms:** Columns too narrow, too wide, or not responsive

**Solutions:**
1. Set column widths explicitly:
   ```javascript
   columnDefs: [
     { field: 'name', width: 200 },
     { field: 'description', width: '*' }, // Takes remaining space
     { field: 'date', width: 120 }
   ]
   ```

2. Set minimum/maximum widths:
   ```javascript
   columnDefs: [
     { field: 'name', minWidth: 100, maxWidth: 300 }
   ]
   ```

3. Enable column resizing:
   ```javascript
   $scope.gridOptions = {
     enableColumnResizing: true
   };
   ```

### Horizontal Scrolling Issues

**Solutions:**
1. Set a minimum grid width:
   ```css
   .grid {
     min-width: 800px;
   }
   ```

2. Use percentage widths for responsive design:
   ```javascript
   columnDefs: [
     { field: 'name', width: '30%' },
     { field: 'email', width: '40%' },
     { field: 'phone', width: '30%' }
   ]
   ```

## Functionality Issues

### Sorting Not Working

**Symptoms:** Clicking column headers doesn't sort data

**Solutions:**
1. Ensure sorting is enabled:
   ```javascript
   $scope.gridOptions = {
     enableSorting: true
   };
   ```

2. Check column-specific settings:
   ```javascript
   columnDefs: [
     { field: 'name', enableSorting: true },
     { field: 'calculated', enableSorting: false } // Disable for specific column
   ]
   ```

3. For custom data types, provide sort function:
   ```javascript
   columnDefs: [
     {
       field: 'customField',
       sortingAlgorithm: function(a, b) {
         // Custom sorting logic
         return a.localeCompare(b);
       }
     }
   ]
   ```

### Filtering Not Working

**Symptoms:** Filter inputs don't affect displayed data

**Solutions:**
1. Enable filtering globally:
   ```javascript
   $scope.gridOptions = {
     enableFiltering: true
   };
   ```

2. Check data types:
   ```javascript
   columnDefs: [
     { field: 'age', type: 'number' }, // Important for numeric filtering
     { field: 'date', type: 'date' }   // Important for date filtering
   ]
   ```

3. For custom filtering:
   ```javascript
   columnDefs: [
     {
       field: 'status',
       filter: {
         condition: function(searchTerm, cellValue) {
           return cellValue.toLowerCase().indexOf(searchTerm.toLowerCase()) >= 0;
         }
       }
     }
   ]
   ```

### Selection Not Working

**Symptoms:** Rows can't be selected or selection events not firing

**Solutions:**
1. Include selection module:
   ```javascript
   angular.module('myApp', ['ui.grid', 'ui.grid.selection']);
   ```

2. Add directive to HTML:
   ```html
   <div ui-grid="gridOptions" ui-grid-selection></div>
   ```

3. Enable selection in options:
   ```javascript
   $scope.gridOptions = {
     enableRowSelection: true,
     multiSelect: true
   };
   ```

### Editing Not Working

**Symptoms:** Cells not editable or edit events not firing

**Solutions:**
1. Include edit module:
   ```javascript
   angular.module('myApp', ['ui.grid', 'ui.grid.edit']);
   ```

2. Add directive to HTML:
   ```html
   <div ui-grid="gridOptions" ui-grid-edit></div>
   ```

3. Enable editing for columns:
   ```javascript
   columnDefs: [
     { field: 'name', enableCellEdit: true },
     { field: 'readonly', enableCellEdit: false }
   ]
   ```

## Data Issues

### Data Not Displaying

**Symptoms:** Grid shows headers but no data rows

**Solutions:**
1. Check data format:
   ```javascript
   // Correct format
   $scope.gridOptions.data = [
     { field1: 'value1', field2: 'value2' },
     { field1: 'value3', field2: 'value4' }
   ];
   ```

2. Verify async data loading:
   ```javascript
   $http.get('/api/data').then(function(response) {
     $scope.gridOptions.data = response.data; // Make sure this executes
   });
   ```

3. Check for JavaScript errors in console

### Data Not Updating

**Symptoms:** Grid doesn't reflect data changes

**Solutions:**
1. For reference changes, assign new array:
   ```javascript
   // Instead of modifying existing array
   $scope.gridOptions.data.push(newItem);
   
   // Create new array reference
   $scope.gridOptions.data = $scope.gridOptions.data.concat([newItem]);
   ```

2. Use grid API to refresh:
   ```javascript
   $scope.gridApi.core.notifyDataChange('data');
   ```

3. For external data changes:
   ```javascript
   $scope.$apply(); // If changes happen outside Angular context
   ```

## CSS and Styling Issues

### Custom Styles Not Applied

**Solutions:**
1. Increase CSS specificity:
   ```css
   .my-grid .ui-grid-cell {
     /* Your custom styles */
   }
   ```

2. Use `!important` as last resort:
   ```css
   .custom-cell {
     background-color: red !important;
   }
   ```

3. Load custom CSS after UI-Grid CSS

### Print Styles Not Working

**Solutions:**
1. Include print-specific CSS:
   ```css
   @media print {
     .ui-grid {
       height: auto !important;
     }
     .ui-grid-viewport {
       height: auto !important;
       overflow: visible !important;
     }
   }
   ```

## Browser-Specific Issues

### Internet Explorer Issues

**Common IE problems and solutions:**

1. **Missing Array methods:**
   ```html
   <!-- Include polyfill for IE -->
   <script src="https://polyfill.io/v3/polyfill.min.js"></script>
   ```

2. **Flexbox issues:**
   ```css
   .ui-grid-cell {
     display: block; /* Fallback for IE */
   }
   ```

### Mobile Browser Issues

**Solutions:**
1. Add viewport meta tag:
   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1">
   ```

2. Enable touch scrolling:
   ```css
   .ui-grid-viewport {
     -webkit-overflow-scrolling: touch;
   }
   ```

## Debugging Tips

### Enable Debug Mode

```javascript
$scope.gridOptions = {
  debug: true // Enables console logging
};
```

### Inspect Grid API

```javascript
$scope.gridOptions = {
  onRegisterApi: function(gridApi) {
    $scope.gridApi = gridApi;
    console.log('Grid API:', gridApi); // Inspect available methods
    window.gridApi = gridApi; // Make available in browser console
  }
};
```

### Check Grid State

```javascript
// In browser console
gridApi.grid.columns.forEach(function(col) {
  console.log(col.field, col.width, col.visible);
});

console.log('Visible rows:', gridApi.core.getVisibleRows().length);
console.log('All data:', gridApi.grid.options.data.length);
```

### Performance Profiling

```javascript
// Time grid operations
console.time('grid-render');
$scope.gridOptions.data = largeDataset;
$timeout(function() {
  console.timeEnd('grid-render');
});
```

## Getting Help

If you're still experiencing issues:

1. **Check the browser console** for JavaScript errors
2. **Create a minimal reproduction** of your issue
3. **Search existing issues** on [GitHub](https://github.com/angular-ui/ng-grid/issues)
4. **Ask questions** with specific details about your setup
5. **Provide sample code** when reporting bugs

## Common Error Messages

### Template Errors

```
Error: [$compile:tpload] Failed to load template
```
**Solution:** Ensure UI-Grid CSS is included for default templates.

### Digest Errors

```
Error: [$rootScope:infdig] 10 $digest() iterations reached
```
**Solution:** Avoid creating new objects in cell templates or use `track by`.

### Memory Errors

```
Error: Maximum call stack size exceeded
```
**Solution:** Check for circular references in data or infinite loops in custom functions.