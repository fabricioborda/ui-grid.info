# Examples and Tutorials

This section contains working examples and step-by-step tutorials for common UI-Grid scenarios.

## Basic Examples

### 1. Simple Grid

A minimal grid setup with basic data display.

```html
<div ng-controller="SimpleGridCtrl">
  <div ui-grid="gridOptions" class="grid"></div>
</div>
```

```javascript
angular.module('app').controller('SimpleGridCtrl', function($scope) {
  $scope.gridOptions = {
    data: [
      { name: 'Alice', age: 30, city: 'New York' },
      { name: 'Bob', age: 25, city: 'London' },
      { name: 'Charlie', age: 35, city: 'Paris' }
    ]
  };
});
```

### 2. Custom Columns

Defining custom column properties and formatting.

```javascript
$scope.gridOptions = {
  columnDefs: [
    { 
      field: 'firstName', 
      displayName: 'First Name',
      width: 150 
    },
    { 
      field: 'lastName', 
      displayName: 'Last Name',
      width: 150 
    },
    { 
      field: 'salary', 
      displayName: 'Salary',
      type: 'number',
      cellFilter: 'currency',
      width: 120
    },
    { 
      field: 'joinDate', 
      displayName: 'Join Date',
      type: 'date',
      cellFilter: 'date:"MM/dd/yyyy"',
      width: 110
    }
  ],
  data: employees
};
```

### 3. Sorting and Filtering

Enable interactive sorting and filtering.

```javascript
$scope.gridOptions = {
  enableSorting: true,
  enableFiltering: true,
  columnDefs: [
    { field: 'name', filter: { placeholder: 'Filter names...' } },
    { 
      field: 'age', 
      filters: [
        { condition: uiGridConstants.filter.GREATER_THAN, placeholder: 'Min' },
        { condition: uiGridConstants.filter.LESS_THAN, placeholder: 'Max' }
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
  ],
  data: myData
};
```

## Advanced Examples

### 4. Row Selection

Multiple row selection with event handling.

```javascript
angular.module('app').controller('SelectionCtrl', function($scope) {
  $scope.gridOptions = {
    enableRowSelection: true,
    enableSelectAll: true,
    multiSelect: true,
    data: myData,
    onRegisterApi: function(gridApi) {
      $scope.gridApi = gridApi;
      
      gridApi.selection.on.rowSelectionChanged($scope, function(row) {
        console.log('Row selected:', row.entity);
        $scope.selectedRow = row.entity;
      });
      
      gridApi.selection.on.rowSelectionChangedBatch($scope, function(rows) {
        console.log('Rows selected:', rows.length);
        $scope.selectedRows = gridApi.selection.getSelectedRows();
      });
    }
  };
  
  $scope.selectAll = function() {
    $scope.gridApi.selection.selectAllRows();
  };
  
  $scope.clearSelection = function() {
    $scope.gridApi.selection.clearSelectedRows();
  };
});
```

```html
<div ng-controller="SelectionCtrl">
  <button ng-click="selectAll()">Select All</button>
  <button ng-click="clearSelection()">Clear Selection</button>
  <div ui-grid="gridOptions" ui-grid-selection class="grid"></div>
  <p>Selected: {{selectedRows.length}} rows</p>
</div>
```

### 5. Inline Editing

Editable grid with validation.

```javascript
angular.module('app').controller('EditCtrl', function($scope) {
  $scope.gridOptions = {
    enableCellEditOnFocus: true,
    columnDefs: [
      { 
        field: 'name',
        enableCellEdit: true,
        validators: { required: true },
        cellEditableCondition: true
      },
      { 
        field: 'age',
        type: 'number',
        enableCellEdit: true,
        validators: { required: true, minValue: 0, maxValue: 150 }
      },
      { 
        field: 'gender',
        enableCellEdit: true,
        editType: 'dropdown',
        editDropdownOptionsArray: [
          { id: 'male', value: 'Male' },
          { id: 'female', value: 'Female' }
        ]
      },
      {
        field: 'isActive',
        type: 'boolean',
        enableCellEdit: true,
        cellTemplate: '<input type="checkbox" ng-model="row.entity.isActive">'
      }
    ],
    data: employees,
    onRegisterApi: function(gridApi) {
      gridApi.edit.on.afterCellEdit($scope, function(rowEntity, colDef, newValue, oldValue) {
        console.log('Cell edited:', colDef.field, oldValue, '->', newValue);
        // Save changes to server
        saveEmployee(rowEntity);
      });
    }
  };
  
  function saveEmployee(employee) {
    // Implementation for saving changes
    console.log('Saving employee:', employee);
  }
});
```

### 6. Custom Cell Templates

Advanced cell rendering with custom templates.

```javascript
$scope.gridOptions = {
  columnDefs: [
    {
      field: 'photo',
      displayName: 'Photo',
      cellTemplate: '<div class="ui-grid-cell-contents"><img src="{{row.entity.photo}}" style="width:40px;height:40px;border-radius:50%"></div>',
      width: 80
    },
    {
      field: 'name',
      displayName: 'Name',
      cellTemplate: '<div class="ui-grid-cell-contents"><strong>{{row.entity.firstName}} {{row.entity.lastName}}</strong></div>'
    },
    {
      field: 'status',
      displayName: 'Status',
      cellTemplate: `
        <div class="ui-grid-cell-contents">
          <span class="badge" ng-class="{
            'badge-success': row.entity.status === 'active',
            'badge-danger': row.entity.status === 'inactive',
            'badge-warning': row.entity.status === 'pending'
          }">
            {{row.entity.status | uppercase}}
          </span>
        </div>
      `
    },
    {
      field: 'actions',
      displayName: 'Actions',
      cellTemplate: `
        <div class="ui-grid-cell-contents">
          <button class="btn btn-sm btn-primary" ng-click="grid.appScope.edit(row.entity)">Edit</button>
          <button class="btn btn-sm btn-danger" ng-click="grid.appScope.delete(row.entity)">Delete</button>
        </div>
      `,
      enableCellEdit: false,
      enableSorting: false,
      enableFiltering: false,
      width: 150
    }
  ],
  data: users
};

$scope.edit = function(entity) {
  console.log('Edit:', entity);
};

$scope.delete = function(entity) {
  if (confirm('Are you sure?')) {
    var index = $scope.gridOptions.data.indexOf(entity);
    $scope.gridOptions.data.splice(index, 1);
  }
};
```

## Feature-Specific Examples

### 7. Pagination

Client-side pagination setup.

```javascript
angular.module('app').controller('PaginationCtrl', function($scope) {
  $scope.gridOptions = {
    paginationPageSizes: [25, 50, 75],
    paginationPageSize: 25,
    useExternalPagination: false,
    data: largeDataSet,
    onRegisterApi: function(gridApi) {
      $scope.gridApi = gridApi;
    }
  };
});
```

```html
<div ng-controller="PaginationCtrl">
  <div ui-grid="gridOptions" ui-grid-pagination class="grid"></div>
</div>
```

### 8. Column Pinning

Pin columns to left or right side.

```javascript
$scope.gridOptions = {
  enablePinning: true,
  columnDefs: [
    { 
      field: 'name', 
      displayName: 'Name',
      pinnedLeft: true,
      width: 150
    },
    { field: 'description', width: 300 },
    { field: 'category', width: 150 },
    { field: 'price', width: 100 },
    { 
      field: 'actions',
      displayName: 'Actions',
      pinnedRight: true,
      width: 120,
      cellTemplate: '<button class="btn btn-sm btn-primary">Action</button>'
    }
  ],
  data: products
};
```

### 9. Grouping

Group rows by column values.

```javascript
angular.module('app').controller('GroupingCtrl', function($scope) {
  $scope.gridOptions = {
    enableGrouping: true,
    treeRowHeaderAlwaysVisible: false,
    columnDefs: [
      { field: 'name', grouping: { groupPriority: 0 } },
      { field: 'department', grouping: { groupPriority: 1 } },
      { field: 'position' },
      { field: 'salary', type: 'number', cellFilter: 'currency' }
    ],
    data: employees
  };
});
```

### 10. Export Functionality

Export grid data to various formats.

```javascript
angular.module('app').controller('ExportCtrl', function($scope, uiGridExporterConstants) {
  $scope.gridOptions = {
    enableGridMenu: true,
    gridMenuCustomItems: [
      {
        title: 'Export CSV',
        action: function() {
          $scope.gridApi.exporter.csvExport('all', 'all');
        }
      },
      {
        title: 'Export PDF',
        action: function() {
          $scope.gridApi.exporter.pdfExport('all', 'all');
        }
      }
    ],
    exporterCsvFilename: 'myData.csv',
    exporterPdfFilename: 'myData.pdf',
    exporterCsvLinkElement: angular.element(document.querySelectorAll(".custom-csv-link-location")),
    onRegisterApi: function(gridApi) {
      $scope.gridApi = gridApi;
    },
    data: myData
  };
  
  $scope.exportCsv = function() {
    $scope.gridApi.exporter.csvExport('all', 'all');
  };
  
  $scope.exportPdf = function() {
    $scope.gridApi.exporter.pdfExport('all', 'all');
  };
});
```

## CSS Styling Examples

### Custom Grid Styling

```css
/* Custom grid theme */
.my-grid {
  border: 2px solid #337ab7;
  border-radius: 4px;
}

.my-grid .ui-grid-header {
  background-color: #337ab7;
  color: white;
  font-weight: bold;
}

.my-grid .ui-grid-row:nth-child(odd) .ui-grid-cell {
  background-color: #f9f9f9;
}

.my-grid .ui-grid-row:hover .ui-grid-cell {
  background-color: #e6f3ff;
}

/* Custom cell styling */
.status-active {
  color: #5cb85c;
  font-weight: bold;
}

.status-inactive {
  color: #d9534f;
  font-weight: bold;
}

.currency-cell {
  text-align: right;
  font-family: 'Courier New', monospace;
}

.action-buttons .btn {
  margin-right: 5px;
}
```

## Complete Working Example

### Employee Management System

```html
<!DOCTYPE html>
<html ng-app="employeeApp">
<head>
    <title>Employee Management</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <style>
        .grid { width: 100%; height: 500px; }
        .toolbar { margin-bottom: 15px; }
        .status-badge { padding: 3px 8px; border-radius: 3px; font-size: 11px; }
        .status-active { background-color: #d4edda; color: #155724; }
        .status-inactive { background-color: #f8d7da; color: #721c24; }
    </style>
</head>
<body ng-controller="MainCtrl">
    <div class="container">
        <h1>Employee Management System</h1>
        
        <div class="toolbar">
            <button class="btn btn-primary" ng-click="addEmployee()">Add Employee</button>
            <button class="btn btn-success" ng-click="exportCsv()">Export CSV</button>
            <span class="pull-right">
                Selected: {{selectedEmployees.length}} employees
            </span>
        </div>
        
        <div ui-grid="gridOptions" 
             ui-grid-selection 
             ui-grid-edit 
             ui-grid-pagination 
             ui-grid-exporter 
             class="grid"></div>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.1/angular.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/angular-ui-grid@4.11.1/ui-grid.min.js"></script>
    <script>
        angular.module('employeeApp', ['ui.grid', 'ui.grid.selection', 'ui.grid.edit', 'ui.grid.pagination', 'ui.grid.exporter'])
        .controller('MainCtrl', function($scope) {
            $scope.selectedEmployees = [];
            
            $scope.gridOptions = {
                enableRowSelection: true,
                enableSelectAll: true,
                multiSelect: true,
                enableFiltering: true,
                enableSorting: true,
                paginationPageSizes: [10, 25, 50],
                paginationPageSize: 25,
                columnDefs: [
                    { 
                        field: 'id', 
                        displayName: 'ID', 
                        width: 60,
                        enableCellEdit: false 
                    },
                    { 
                        field: 'firstName', 
                        displayName: 'First Name',
                        width: 120,
                        enableCellEdit: true
                    },
                    { 
                        field: 'lastName', 
                        displayName: 'Last Name',
                        width: 120,
                        enableCellEdit: true
                    },
                    { 
                        field: 'email', 
                        displayName: 'Email',
                        width: 200,
                        enableCellEdit: true
                    },
                    { 
                        field: 'department', 
                        displayName: 'Department',
                        width: 120,
                        enableCellEdit: true,
                        editType: 'dropdown',
                        editDropdownOptionsArray: [
                            'Engineering', 'Marketing', 'Sales', 'HR', 'Finance'
                        ]
                    },
                    { 
                        field: 'salary', 
                        displayName: 'Salary',
                        type: 'number',
                        cellFilter: 'currency',
                        width: 120,
                        enableCellEdit: true
                    },
                    {
                        field: 'status',
                        displayName: 'Status',
                        width: 100,
                        cellTemplate: '<div class="ui-grid-cell-contents"><span class="status-badge status-{{row.entity.status}}">{{row.entity.status | uppercase}}</span></div>',
                        enableCellEdit: true,
                        editType: 'dropdown',
                        editDropdownOptionsArray: ['active', 'inactive']
                    },
                    {
                        field: 'actions',
                        displayName: 'Actions',
                        cellTemplate: '<div class="ui-grid-cell-contents"><button class="btn btn-xs btn-danger" ng-click="grid.appScope.deleteEmployee(row.entity)">Delete</button></div>',
                        enableCellEdit: false,
                        enableSorting: false,
                        enableFiltering: false,
                        width: 80
                    }
                ],
                data: [
                    { id: 1, firstName: 'John', lastName: 'Doe', email: 'john.doe@company.com', department: 'Engineering', salary: 75000, status: 'active' },
                    { id: 2, firstName: 'Jane', lastName: 'Smith', email: 'jane.smith@company.com', department: 'Marketing', salary: 68000, status: 'active' },
                    { id: 3, firstName: 'Bob', lastName: 'Johnson', email: 'bob.johnson@company.com', department: 'Sales', salary: 72000, status: 'inactive' },
                    { id: 4, firstName: 'Alice', lastName: 'Brown', email: 'alice.brown@company.com', department: 'HR', salary: 65000, status: 'active' },
                    { id: 5, firstName: 'Charlie', lastName: 'Wilson', email: 'charlie.wilson@company.com', department: 'Finance', salary: 78000, status: 'active' }
                ],
                onRegisterApi: function(gridApi) {
                    $scope.gridApi = gridApi;
                    
                    gridApi.selection.on.rowSelectionChangedBatch($scope, function(rows) {
                        $scope.selectedEmployees = gridApi.selection.getSelectedRows();
                    });
                    
                    gridApi.edit.on.afterCellEdit($scope, function(rowEntity, colDef, newValue, oldValue) {
                        console.log('Employee updated:', rowEntity);
                    });
                }
            };
            
            $scope.addEmployee = function() {
                var newId = Math.max.apply(Math, $scope.gridOptions.data.map(function(e) { return e.id; })) + 1;
                $scope.gridOptions.data.push({
                    id: newId,
                    firstName: 'New',
                    lastName: 'Employee',
                    email: 'new.employee@company.com',
                    department: 'Engineering',
                    salary: 70000,
                    status: 'active'
                });
            };
            
            $scope.deleteEmployee = function(employee) {
                if (confirm('Are you sure you want to delete ' + employee.firstName + ' ' + employee.lastName + '?')) {
                    var index = $scope.gridOptions.data.indexOf(employee);
                    $scope.gridOptions.data.splice(index, 1);
                }
            };
            
            $scope.exportCsv = function() {
                $scope.gridApi.exporter.csvExport('all', 'all');
            };
        });
    </script>
</body>
</html>
```

This complete example demonstrates:
- Row selection with multi-select
- Inline editing with dropdowns
- Pagination
- Filtering and sorting
- Custom cell templates
- Action buttons
- CSV export
- Professional styling

Copy this code into an HTML file and open it in your browser to see a fully functional employee management system built with UI-Grid.