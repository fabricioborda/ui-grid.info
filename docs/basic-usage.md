# Basic Usage

This guide covers the fundamental concepts and basic usage patterns for UI-Grid.

## Core Concepts

### Grid Options

The `gridOptions` object is the main configuration point for UI-Grid:

```javascript
$scope.gridOptions = {
  // Data source
  data: myData,
  
  // Column definitions
  columnDefs: [
    { field: 'name', displayName: 'Full Name' },
    { field: 'age', type: 'number' },
    { field: 'email', type: 'email' }
  ],
  
  // Grid features
  enableSorting: true,
  enableFiltering: true,
  enableGridMenu: true
};
```

### Column Definitions

Columns can be defined explicitly or auto-generated:

```javascript
// Explicit column definitions
$scope.gridOptions = {
  columnDefs: [
    {
      field: 'name',
      displayName: 'Full Name',
      width: 200,
      minWidth: 100,
      maxWidth: 300
    },
    {
      field: 'birthDate',
      displayName: 'Birth Date',
      type: 'date',
      cellFilter: 'date:"MM/dd/yyyy"'
    },
    {
      field: 'balance',
      displayName: 'Account Balance',
      type: 'number',
      cellFilter: 'currency',
      cellClass: 'text-right'
    }
  ]
};

// Auto-generated (from data properties)
$scope.gridOptions = {
  data: myData // Columns created automatically
};
```

## Data Binding

### Static Data

```javascript
$scope.gridOptions = {
  data: [
    { id: 1, name: 'John Doe', age: 30 },
    { id: 2, name: 'Jane Smith', age: 25 },
    { id: 3, name: 'Bob Johnson', age: 35 }
  ]
};
```

### Dynamic Data

```javascript
// Initially empty
$scope.gridOptions = {
  data: []
};

// Load data asynchronously
$http.get('/api/users').then(function(response) {
  $scope.gridOptions.data = response.data;
});

// Or bind directly to scope
$scope.myData = [];
$scope.gridOptions = {
  data: 'myData' // String reference
};

$http.get('/api/users').then(function(response) {
  $scope.myData = response.data;
});
```

## Basic Features

### Sorting

Enable sorting globally or per column:

```javascript
$scope.gridOptions = {
  enableSorting: true, // Global
  columnDefs: [
    { field: 'name', enableSorting: true },
    { field: 'age', enableSorting: false }, // Disable for this column
    { 
      field: 'date', 
      sort: { direction: 'desc', priority: 0 } // Default sort
    }
  ]
};
```

### Filtering

Enable filtering with various types:

```javascript
$scope.gridOptions = {
  enableFiltering: true,
  columnDefs: [
    { 
      field: 'name', 
      filter: {
        placeholder: 'Search names...'
      }
    },
    { 
      field: 'age', 
      filters: [
        { condition: uiGridConstants.filter.GREATER_THAN, placeholder: 'Min age' },
        { condition: uiGridConstants.filter.LESS_THAN, placeholder: 'Max age' }
      ]
    },
    {
      field: 'status',
      filter: {
        type: uiGridConstants.filter.SELECT,
        selectOptions: [
          { value: 'active', label: 'Active' },
          { value: 'inactive', label: 'Inactive' }
        ]
      }
    }
  ]
};
```

### Grid Menu

Enable the grid menu for additional options:

```javascript
$scope.gridOptions = {
  enableGridMenu: true,
  gridMenuCustomItems: [
    {
      title: 'Custom Action',
      action: function() {
        alert('Custom menu item clicked');
      }
    }
  ]
};
```

## Styling

### Basic CSS

```css
.grid {
  width: 100%;
  height: 400px;
  border: 1px solid #d4d4d4;
}

.ui-grid-cell {
  padding: 8px;
}

.ui-grid-header-cell {
  font-weight: bold;
  background-color: #f5f5f5;
}
```

### Custom Cell Classes

```javascript
$scope.gridOptions = {
  columnDefs: [
    {
      field: 'status',
      cellClass: function(grid, row, col, rowRenderIndex, colRenderIndex) {
        if (row.entity.status === 'active') {
          return 'text-success';
        } else if (row.entity.status === 'inactive') {
          return 'text-danger';
        }
        return '';
      }
    }
  ]
};
```

## Event Handling

### Grid Events

```javascript
$scope.gridOptions = {
  onRegisterApi: function(gridApi) {
    $scope.gridApi = gridApi;
    
    // Sort changed
    gridApi.core.on.sortChanged($scope, function(grid, sortColumns) {
      console.log('Sort changed', sortColumns);
    });
    
    // Filter changed
    gridApi.core.on.filterChanged($scope, function() {
      console.log('Filter changed');
    });
    
    // Row selection changed
    gridApi.selection.on.rowSelectionChanged($scope, function(row) {
      console.log('Row selected', row.entity);
    });
  }
};
```

### Cell Click Events

```javascript
$scope.gridOptions = {
  columnDefs: [
    {
      field: 'name',
      cellTemplate: '<div class="ui-grid-cell-contents" ng-click="grid.appScope.cellClicked(row)">{{row.entity.name}}</div>'
    }
  ]
};

$scope.cellClicked = function(row) {
  console.log('Cell clicked', row.entity);
};
```

## Common Patterns

### Loading State

```javascript
$scope.loading = true;
$scope.gridOptions = {
  data: []
};

$http.get('/api/data').then(function(response) {
  $scope.gridOptions.data = response.data;
  $scope.loading = false;
});
```

```html
<div ng-show="loading">Loading...</div>
<div ui-grid="gridOptions" ng-hide="loading" class="grid"></div>
```

### Error Handling

```javascript
$http.get('/api/data').then(
  function(response) {
    $scope.gridOptions.data = response.data;
  },
  function(error) {
    $scope.error = 'Failed to load data';
    $scope.gridOptions.data = [];
  }
);
```

### Refresh Data

```javascript
$scope.refreshGrid = function() {
  $scope.loading = true;
  $http.get('/api/data').then(function(response) {
    $scope.gridOptions.data = response.data;
    $scope.loading = false;
  });
};
```

## Performance Tips

1. **Use trackBy** for large datasets:
```javascript
$scope.gridOptions = {
  rowIdentity: function(row) {
    return row.id;
  }
};
```

2. **Disable features** you don't need:
```javascript
$scope.gridOptions = {
  enableSorting: false,
  enableFiltering: false,
  enableColumnMenus: false
};
```

3. **Use virtualization** for large datasets (enabled by default).

## Next Steps

- [Advanced Configuration](./advanced-configuration.md) - More complex setups
- [API Reference](./api-reference.md) - Complete API documentation
- [Examples](./examples/README.md) - Working examples