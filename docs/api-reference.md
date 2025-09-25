# API Reference

Complete API documentation for UI-Grid components, services, and configuration options.

## Grid Options

The main configuration object for UI-Grid.

### Core Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `data` | Array or String | `[]` | Data array or scope property name |
| `columnDefs` | Array | `[]` | Column definition objects |
| `enableSorting` | Boolean | `true` | Enable column sorting |
| `enableFiltering` | Boolean | `false` | Enable column filtering |
| `enableGridMenu` | Boolean | `false` | Enable grid menu |
| `enableColumnMenus` | Boolean | `true` | Enable column menus |
| `showGridFooter` | Boolean | `false` | Show grid footer |
| `showColumnFooter` | Boolean | `false` | Show column footer |
| `virtualizationThreshold` | Number | `20` | Row count threshold for virtualization |
| `rowHeight` | Number | `30` | Height of each row in pixels |
| `headerRowHeight` | Number | `30` | Height of header row |
| `footerRowHeight` | Number | `30` | Height of footer row |

### Event Callbacks

| Property | Type | Description |
|----------|------|-------------|
| `onRegisterApi` | Function | Called when grid API is available |

### Example

```javascript
$scope.gridOptions = {
  data: myData,
  enableSorting: true,
  enableFiltering: true,
  enableGridMenu: true,
  columnDefs: [
    { field: 'name', displayName: 'Name' },
    { field: 'age', displayName: 'Age', type: 'number' }
  ],
  onRegisterApi: function(gridApi) {
    $scope.gridApi = gridApi;
  }
};
```

## Column Definition

Configuration for individual columns.

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `field` | String | Required | Property name in data object |
| `displayName` | String | `field` | Display name for column header |
| `name` | String | `field` | Unique column identifier |
| `type` | String | `'string'` | Data type (string, number, boolean, date, object) |
| `width` | Number or String | `'*'` | Column width |
| `minWidth` | Number | `30` | Minimum column width |
| `maxWidth` | Number | `9000` | Maximum column width |
| `visible` | Boolean | `true` | Column visibility |
| `sort` | Object | `{}` | Default sort configuration |
| `filters` | Array | `[]` | Filter configurations |
| `cellTemplate` | String | | Custom cell template HTML |
| `cellClass` | String or Function | | CSS class(es) for cells |
| `headerCellTemplate` | String | | Custom header template HTML |
| `headerCellClass` | String or Function | | CSS class(es) for header |
| `cellFilter` | String | | Angular filter expression |
| `enableSorting` | Boolean | `true` | Enable sorting for this column |
| `enableFiltering` | Boolean | `true` | Enable filtering for this column |
| `enableHiding` | Boolean | `true` | Allow hiding via column menu |
| `enableColumnMenu` | Boolean | `true` | Show column menu |

### Example

```javascript
columnDefs: [
  {
    field: 'name',
    displayName: 'Full Name',
    width: 200,
    minWidth: 100,
    cellClass: 'text-left',
    headerCellClass: 'header-center',
    enableSorting: true,
    enableFiltering: true
  },
  {
    field: 'birthDate',
    displayName: 'Birth Date',
    type: 'date',
    cellFilter: 'date:"MM/dd/yyyy"',
    width: 120
  },
  {
    field: 'salary',
    displayName: 'Salary',
    type: 'number',
    cellFilter: 'currency',
    cellClass: 'text-right',
    sort: { direction: 'desc', priority: 1 }
  }
]
```

## Grid API

The Grid API provides programmatic access to grid functionality.

### Core API

#### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `refresh()` | | | Refresh the entire grid |
| `refreshRows()` | | | Refresh row rendering |
| `handleWindowResize()` | | | Handle window resize |
| `addRowHeaderColumn(colDef)` | colDef | | Add row header column |
| `scrollToIfNecessary(gridRow, gridCol)` | gridRow, gridCol | Promise | Scroll to make row/col visible |
| `getVisibleRows()` | | Array | Get visible row entities |

#### Events

| Event | Parameters | Description |
|-------|------------|-------------|
| `sortChanged` | grid, sortColumns | Fired when sort changes |
| `filterChanged` | | Fired when filter changes |
| `columnVisibilityChanged` | changedColumn | Fired when column visibility changes |
| `rowsVisibleChanged` | | Fired when visible rows change |

### Selection API

Available when `ui.grid.selection` module is included.

#### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `selectRow(row)` | row | | Select a row |
| `selectRowByVisibleIndex(index)` | index | | Select row by visible index |
| `unSelectRow(row)` | row | | Unselect a row |
| `selectAllRows()` | | | Select all rows |
| `clearSelectedRows()` | | | Clear all selections |
| `getSelectedRows()` | | Array | Get selected row entities |
| `getSelectedGridRows()` | | Array | Get selected grid rows |
| `setMultiSelect(multiSelect)` | boolean | | Enable/disable multi-select |
| `setModifierKeysToMultiSelect(keys)` | boolean | | Use modifier keys for multi-select |
| `getSelectAllState()` | | String | Get select all checkbox state |

#### Events

| Event | Parameters | Description |
|-------|------------|-------------|
| `rowSelectionChanged` | row | Fired when row selection changes |
| `rowSelectionChangedBatch` | rows | Fired when multiple rows change |

### Edit API

Available when `ui.grid.edit` module is included.

#### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `getColumnEditor(column)` | column | String | Get editor template for column |

#### Events

| Event | Parameters | Description |
|-------|------------|-------------|
| `afterCellEdit` | rowEntity, column, newValue, oldValue | After cell edit |
| `beginCellEdit` | rowEntity, column | Before cell edit begins |
| `cancelCellEdit` | rowEntity, column | When cell edit is cancelled |

### Pagination API

Available when `ui.grid.pagination` module is included.

#### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `seek(page)` | page | | Go to specific page |
| `nextPage()` | | | Go to next page |
| `previousPage()` | | | Go to previous page |
| `getTotalPages()` | | Number | Get total page count |
| `getPage()` | | Number | Get current page number |

### CellNav API

Available when `ui.grid.cellNav` module is included.

#### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `getFocusedCell()` | | Object | Get currently focused cell |
| `getCurrentSelection()` | | Array | Get current cell selection |
| `rowColSelectIndex(index)` | index | | Select cell by index |

## Constants

UI-Grid provides constants for various configurations.

### uiGridConstants

```javascript
// Filter conditions
uiGridConstants.filter.STARTS_WITH
uiGridConstants.filter.ENDS_WITH
uiGridConstants.filter.EXACT
uiGridConstants.filter.CONTAINS
uiGridConstants.filter.GREATER_THAN
uiGridConstants.filter.GREATER_THAN_OR_EQUAL
uiGridConstants.filter.LESS_THAN
uiGridConstants.filter.LESS_THAN_OR_EQUAL
uiGridConstants.filter.NOT_EQUAL

// Data types
uiGridConstants.dataTypes.STRING
uiGridConstants.dataTypes.NUMBER
uiGridConstants.dataTypes.DATE
uiGridConstants.dataTypes.BOOLEAN
uiGridConstants.dataTypes.OBJECT

// Key codes
uiGridConstants.keymap.TAB
uiGridConstants.keymap.ENTER
uiGridConstants.keymap.ESC
uiGridConstants.keymap.LEFT
uiGridConstants.keymap.RIGHT
uiGridConstants.keymap.UP
uiGridConstants.keymap.DOWN
```

## Custom Templates

### Cell Templates

```javascript
columnDefs: [
  {
    field: 'name',
    cellTemplate: '<div class="ui-grid-cell-contents"><strong>{{row.entity.name}}</strong></div>'
  },
  {
    field: 'action',
    cellTemplate: '<button class="btn btn-primary" ng-click="grid.appScope.edit(row.entity)">Edit</button>'
  }
]
```

### Header Templates

```javascript
columnDefs: [
  {
    field: 'name',
    headerCellTemplate: '<div class="ui-grid-cell-contents custom-header">{{col.displayName}}</div>'
  }
]
```

### Footer Templates

```javascript
$scope.gridOptions = {
  showColumnFooter: true,
  columnDefs: [
    {
      field: 'amount',
      footerCellTemplate: '<div class="ui-grid-cell-contents">Total: {{grid.appScope.getTotal()}}</div>'
    }
  ]
};
```

## Error Handling

Common error scenarios and solutions:

### Module Not Found
```
Error: [$injector:modulerr] Failed to instantiate module ui.grid
```
**Solution**: Ensure ui-grid.js is loaded before your module.

### Template Not Found
```
Error: [$templateRequest:tpload] Error loading template
```
**Solution**: Include ui-grid.css for default templates.

### Performance Issues
- Use `rowIdentity` for large datasets
- Disable unused features
- Consider server-side pagination for very large datasets

## Next Steps

- [Advanced Configuration](./advanced-configuration.md) - Complex setups
- [Examples](./examples/README.md) - Working code examples
- [Developer Guide](./developer-guide.md) - Contributing to UI-Grid