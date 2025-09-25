# Advanced Configuration

Advanced configuration options and patterns for complex UI-Grid implementations.

## Grid-Level Configuration

### Performance Optimization

```javascript
$scope.gridOptions = {
  // Virtualization settings
  virtualizationThreshold: 20,
  scrollDebounce: 300,
  wheelScrollThrottle: 70,
  
  // Row settings
  rowHeight: 30,
  headerRowHeight: 30,
  footerRowHeight: 30,
  
  // Improve performance for large datasets
  rowIdentity: function(row) {
    return row.id; // Use unique identifier
  },
  
  // Disable features you don't need
  enableColumnMenus: false,
  enableSorting: true,
  enableFiltering: false,
  enableGridMenu: false
};
```

### Custom Grid Classes

```javascript
$scope.gridOptions = {
  rowTemplate: '<div ng-class="{ \'selected\': row.isSelected, \'even\': $even, \'odd\': $odd }" ng-repeat="(colRenderIndex, col) in colContainer.renderedColumns track by col.uid" ui-grid-one-bind-id-grid="rowRenderIndex + \'-\' + col.uid + \'-cell\'" class="ui-grid-cell" ng-class="{ \'ui-grid-row-header-cell\': col.isRowHeader }" role="{{col.isRowHeader ? \'rowheader\' : \'gridcell\'}}" ui-grid-cell></div>',
  
  gridMenuCustomItems: [
    {
      title: 'Custom Action',
      icon: 'ui-grid-icon-info-circled',
      action: function($event) {
        // Custom action logic
      }
    }
  ]
};
```

## Column-Level Configuration

### Advanced Column Definitions

```javascript
$scope.gridOptions = {
  columnDefs: [
    {
      field: 'name',
      displayName: 'Full Name',
      
      // Column visibility and sizing
      visible: true,
      width: 200,
      minWidth: 100,
      maxWidth: 300,
      
      // Sorting configuration
      enableSorting: true,
      sortDirectionCycle: [null, 'asc', 'desc'],
      sortingAlgorithm: function(a, b, rowA, rowB, direction) {
        return a.localeCompare(b);
      },
      
      // Filtering configuration
      enableFiltering: true,
      filter: {
        condition: uiGridConstants.filter.CONTAINS,
        placeholder: 'Search names...',
        ariaLabel: 'Filter for name'
      },
      
      // Cell configuration
      cellTooltip: function(row, col) {
        return 'Name: ' + row.entity[col.field];
      },
      cellClass: function(grid, row, col, rowRenderIndex, colRenderIndex) {
        if (row.entity.isVip) {
          return 'vip-customer';
        }
      },
      
      // Header configuration
      headerTooltip: 'Customer full name',
      headerCellClass: 'text-center',
      
      // Menu configuration
      enableColumnMenu: true,
      menuItems: [
        {
          title: 'Custom Menu Item',
          icon: 'ui-grid-icon-info-circled',
          action: function($event) {
            // Custom menu action
          }
        }
      ]
    }
  ]
};
```

### Data Type Configurations

```javascript
$scope.gridOptions = {
  columnDefs: [
    // String with custom formatting
    {
      field: 'name',
      type: 'string',
      cellFilter: 'uppercase'
    },
    
    // Number with precision and currency
    {
      field: 'salary',
      type: 'number',
      cellFilter: 'currency:"$":2',
      cellClass: 'text-right'
    },
    
    // Date with custom format
    {
      field: 'birthDate',
      type: 'date',
      cellFilter: 'date:"MM/dd/yyyy"',
      sortingAlgorithm: function(a, b) {
        return new Date(a) - new Date(b);
      }
    },
    
    // Boolean with custom display
    {
      field: 'isActive',
      type: 'boolean',
      cellTemplate: '<div class="ui-grid-cell-contents"><span class="badge badge-{{row.entity.isActive ? \'success\' : \'danger\'}}">{{row.entity.isActive ? \'Active\' : \'Inactive\'}}</span></div>'
    },
    
    // Object field with nested access
    {
      field: 'address.city',
      displayName: 'City',
      type: 'string'
    },
    
    // Custom data type
    {
      field: 'priority',
      type: 'custom',
      sortingAlgorithm: function(a, b) {
        var priorities = { 'High': 3, 'Medium': 2, 'Low': 1 };
        return priorities[b] - priorities[a];
      },
      cellTemplate: '<div class="ui-grid-cell-contents priority-{{row.entity.priority.toLowerCase()}}">{{row.entity.priority}}</div>'
    }
  ]
};
```

## Feature Module Configuration

### Selection Module

```javascript
angular.module('app', ['ui.grid', 'ui.grid.selection']);

$scope.gridOptions = {
  enableRowSelection: true,
  enableRowHeaderSelection: false,
  multiSelect: true,
  modifierKeysToMultiSelect: false,
  noUnselect: false,
  enableSelectAll: true,
  enableSelectionBatchEvent: true,
  
  // Selection behavior
  isRowSelectable: function(row) {
    return row.entity.status === 'active';
  },
  
  onRegisterApi: function(gridApi) {
    $scope.gridApi = gridApi;
    
    // Selection events
    gridApi.selection.on.rowSelectionChanged($scope, function(row) {
      console.log('Row selection changed:', row.entity);
    });
    
    gridApi.selection.on.rowSelectionChangedBatch($scope, function(rows) {
      console.log('Batch selection changed:', rows.length);
    });
  }
};
```

### Edit Module

```javascript
angular.module('app', ['ui.grid', 'ui.grid.edit']);

$scope.gridOptions = {
  enableCellEditOnFocus: true,
  cellEditableCondition: function($scope) {
    return $scope.row.entity.editable !== false;
  },
  
  columnDefs: [
    {
      field: 'name',
      enableCellEdit: true,
      validators: {
        required: true,
        minLength: 2,
        maxLength: 50
      },
      cellEditableCondition: function($scope) {
        return $scope.row.entity.status === 'draft';
      }
    },
    {
      field: 'category',
      enableCellEdit: true,
      editType: 'dropdown',
      editDropdownIdLabel: 'id',
      editDropdownValueLabel: 'value',
      editDropdownOptionsArray: [
        { id: 1, value: 'Category A' },
        { id: 2, value: 'Category B' },
        { id: 3, value: 'Category C' }
      ]
    },
    {
      field: 'priority',
      enableCellEdit: true,
      editType: 'dropdown',
      editDropdownOptionsArray: ['High', 'Medium', 'Low']
    },
    {
      field: 'startDate',
      type: 'date',
      enableCellEdit: true,
      cellFilter: 'date:"MM/dd/yyyy"'
    }
  ],
  
  onRegisterApi: function(gridApi) {
    gridApi.edit.on.afterCellEdit($scope, function(rowEntity, colDef, newValue, oldValue) {
      if (newValue !== oldValue) {
        // Save changes
        saveRowData(rowEntity);
      }
    });
    
    gridApi.edit.on.beginCellEdit($scope, function(rowEntity, colDef) {
      console.log('Begin editing:', colDef.field);
    });
    
    gridApi.edit.on.cancelCellEdit($scope, function(rowEntity, colDef) {
      console.log('Cancel editing:', colDef.field);
    });
  }
};
```

### Pagination Module

```javascript
angular.module('app', ['ui.grid', 'ui.grid.pagination']);

$scope.gridOptions = {
  // Client-side pagination
  paginationPageSizes: [25, 50, 75, 100],
  paginationPageSize: 25,
  paginationCurrentPage: 1,
  
  // Server-side pagination
  useExternalPagination: true,
  totalItems: 0,
  
  onRegisterApi: function(gridApi) {
    gridApi.pagination.on.paginationChanged($scope, function(newPage, pageSize) {
      if ($scope.gridOptions.useExternalPagination) {
        loadPage(newPage, pageSize);
      }
    });
  }
};

function loadPage(page, pageSize) {
  $http.get('/api/data', {
    params: { page: page, size: pageSize }
  }).then(function(response) {
    $scope.gridOptions.data = response.data.content;
    $scope.gridOptions.totalItems = response.data.totalElements;
  });
}
```

### Infinite Scroll Module

```javascript
angular.module('app', ['ui.grid', 'ui.grid.infiniteScroll']);

$scope.gridOptions = {
  enableInfiniteScroll: true,
  infiniteScrollRowsFromEnd: 20,
  infiniteScrollUp: true,
  infiniteScrollDown: true,
  
  onRegisterApi: function(gridApi) {
    gridApi.infiniteScroll.on.needLoadMoreData($scope, function() {
      loadMoreData().then(function(newData) {
        $scope.gridOptions.data = $scope.gridOptions.data.concat(newData);
        gridApi.infiniteScroll.dataLoaded();
      });
    });
  }
};
```

## Complex Templates

### Multi-Line Cell Content

```javascript
columnDefs: [
  {
    field: 'description',
    displayName: 'Description',
    cellTemplate: `
      <div class="ui-grid-cell-contents multi-line">
        <div class="title">{{row.entity.title}}</div>
        <div class="subtitle">{{row.entity.subtitle}}</div>
        <div class="description">{{row.entity.description | limitTo:100}}{{row.entity.description.length > 100 ? '...' : ''}}</div>
      </div>
    `,
    width: 300
  }
]
```

### Progress Bar Cell

```javascript
columnDefs: [
  {
    field: 'progress',
    displayName: 'Progress',
    cellTemplate: `
      <div class="ui-grid-cell-contents">
        <div class="progress">
          <div class="progress-bar" 
               style="width: {{row.entity.progress}}%" 
               ng-class="{'progress-bar-success': row.entity.progress >= 80, 'progress-bar-warning': row.entity.progress >= 50 && row.entity.progress < 80, 'progress-bar-danger': row.entity.progress < 50}">
            {{row.entity.progress}}%
          </div>
        </div>
      </div>
    `,
    width: 120
  }
]
```

### Image Gallery Cell

```javascript
columnDefs: [
  {
    field: 'images',
    displayName: 'Images',
    cellTemplate: `
      <div class="ui-grid-cell-contents">
        <div class="image-gallery">
          <img ng-repeat="image in row.entity.images | limitTo:3" 
               ng-src="{{image.thumbnail}}" 
               class="thumbnail-image"
               tooltip="{{image.description}}">
          <span ng-if="row.entity.images.length > 3" class="more-images">
            +{{row.entity.images.length - 3}} more
          </span>
        </div>
      </div>
    `,
    width: 150
  }
]
```

## Dynamic Configuration

### Runtime Column Changes

```javascript
$scope.addColumn = function() {
  $scope.gridOptions.columnDefs.push({
    field: 'newField',
    displayName: 'New Column'
  });
};

$scope.removeColumn = function(field) {
  $scope.gridOptions.columnDefs = $scope.gridOptions.columnDefs.filter(function(col) {
    return col.field !== field;
  });
};

$scope.toggleColumn = function(field) {
  var column = $scope.gridApi.grid.getColumn(field);
  column.visible = !column.visible;
  $scope.gridApi.grid.refresh();
};
```

### Conditional Features

```javascript
$scope.gridOptions = {
  enableSorting: $scope.userCanSort,
  enableFiltering: $scope.userCanFilter,
  enableRowSelection: $scope.userCanSelect,
  
  columnDefs: $scope.columns.map(function(col) {
    return {
      field: col.field,
      displayName: col.displayName,
      visible: $scope.userPreferences.visibleColumns.includes(col.field),
      enableCellEdit: $scope.userCanEdit && col.editable,
      width: $scope.userPreferences.columnWidths[col.field] || col.defaultWidth
    };
  })
};
```

## Error Handling

### Validation and Error Display

```javascript
$scope.gridOptions = {
  onRegisterApi: function(gridApi) {
    gridApi.edit.on.afterCellEdit($scope, function(rowEntity, colDef, newValue, oldValue) {
      // Validate the change
      var validation = validateField(colDef.field, newValue, rowEntity);
      
      if (!validation.isValid) {
        // Revert the change
        rowEntity[colDef.field] = oldValue;
        
        // Show error message
        showErrorMessage(validation.message);
        
        // Highlight the cell
        var cellElement = getCellElement(rowEntity, colDef);
        cellElement.addClass('validation-error');
      } else {
        // Save the change
        saveRowData(rowEntity).catch(function(error) {
          // Revert on save error
          rowEntity[colDef.field] = oldValue;
          showErrorMessage('Failed to save: ' + error.message);
        });
      }
    });
  }
};

function validateField(field, value, entity) {
  switch (field) {
    case 'email':
      if (!value || !/\S+@\S+\.\S+/.test(value)) {
        return { isValid: false, message: 'Invalid email address' };
      }
      break;
    case 'age':
      if (value < 0 || value > 150) {
        return { isValid: false, message: 'Age must be between 0 and 150' };
      }
      break;
  }
  return { isValid: true };
}
```

## Integration Patterns

### Form Integration

```javascript
// Sync grid selection with form
$scope.gridOptions = {
  enableRowSelection: true,
  multiSelect: false,
  onRegisterApi: function(gridApi) {
    gridApi.selection.on.rowSelectionChanged($scope, function(row) {
      if (row.isSelected) {
        $scope.selectedItem = angular.copy(row.entity);
      } else {
        $scope.selectedItem = null;
      }
    });
  }
};

$scope.saveForm = function() {
  if ($scope.selectedItem) {
    // Update grid data
    var gridRow = $scope.gridApi.selection.getSelectedRows()[0];
    if (gridRow) {
      angular.extend(gridRow, $scope.selectedItem);
    }
  }
};
```

### Modal Integration

```javascript
$scope.editRow = function(rowEntity) {
  var modalInstance = $uibModal.open({
    templateUrl: 'edit-modal.html',
    controller: 'EditModalCtrl',
    resolve: {
      item: function() {
        return angular.copy(rowEntity);
      }
    }
  });

  modalInstance.result.then(function(updatedItem) {
    // Update the grid
    angular.extend(rowEntity, updatedItem);
  });
};
```

This advanced configuration guide covers complex scenarios and patterns you might encounter when building sophisticated applications with UI-Grid.