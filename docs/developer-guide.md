# Developer Guide

A comprehensive guide for developers working with UI-Grid source code, extending functionality, and building custom features.

## Architecture Overview

### Core Components

UI-Grid is built with a modular architecture consisting of:

1. **Core Module** (`ui.grid.core`): Base grid functionality
2. **Feature Modules**: Optional modules for specific features
3. **Services**: Grid logic and data management
4. **Directives**: DOM manipulation and rendering
5. **Controllers**: Business logic and state management

### Module Structure

```
ui.grid/
├── src/
│   ├── js/
│   │   ├── core/                 # Core functionality
│   │   │   ├── factories/        # Grid factories
│   │   │   ├── services/         # Core services
│   │   │   └── directives/       # Core directives
│   │   └── features/             # Feature modules
│   │       ├── selection/        # Row/cell selection
│   │       ├── edit/             # Inline editing
│   │       ├── pagination/       # Pagination
│   │       └── ...
│   ├── less/                     # LESS stylesheets
│   └── templates/                # HTML templates
└── test/                         # Test files
```

## Core Concepts

### Grid Object

The central `Grid` object manages all grid state:

```javascript
function Grid(options, $log, $q, uiGridConstants) {
  var grid = this;
  
  // Grid properties
  grid.options = options;
  grid.rows = [];
  grid.columns = [];
  grid.renderContainers = {};
  
  // Grid methods
  grid.buildColumns = buildColumns;
  grid.modifyRows = modifyRows;
  grid.sortColumn = sortColumn;
  grid.refresh = refresh;
}
```

### Row and Column Objects

**GridRow:**
```javascript
function GridRow(entity, index, grid) {
  this.grid = grid;
  this.entity = entity;
  this.uid = index;
  this.visible = true;
  this.isSelected = false;
}
```

**GridColumn:**
```javascript
function GridColumn(colDef, uid, grid) {
  this.grid = grid;
  this.uid = uid;
  this.field = colDef.field;
  this.name = colDef.name || colDef.field;
  this.displayName = colDef.displayName || this.name;
  this.width = colDef.width;
  this.visible = colDef.visible !== false;
}
```

### Render Containers

Render containers manage different sections of the grid:

- **body**: Main data area
- **left**: Left pinned columns
- **right**: Right pinned columns
- **header**: Column headers
- **footer**: Column footers

```javascript
grid.renderContainers = {
  body: new GridRenderContainer('body', grid),
  left: new GridRenderContainer('left', grid),
  right: new GridRenderContainer('right', grid)
};
```

## Creating Custom Features

### Feature Module Template

```javascript
(function() {
  'use strict';
  
  /**
   * @ngdoc module
   * @name ui.grid.myFeature
   * @description My custom feature module
   */
  angular.module('ui.grid.myFeature', ['ui.grid']);
  
  /**
   * @ngdoc service
   * @name uiGridMyFeatureService
   * @description Service for my feature
   */
  angular.module('ui.grid.myFeature')
    .service('uiGridMyFeatureService', ['uiGridConstants', function(uiGridConstants) {
      
      var service = {
        initializeGrid: initializeGrid,
        defaultGridOptions: defaultGridOptions
      };
      
      return service;
      
      function defaultGridOptions(gridOptions) {
        return {
          enableMyFeature: true,
          myFeatureSettings: {}
        };
      }
      
      function initializeGrid(grid) {
        // Initialize feature on grid
        grid.api.registerEventsFromObject({
          myFeature: {
            featureEvent: function(grid, args) {}
          }
        });
        
        // Add API methods
        grid.api.registerMethodsFromObject({
          myFeature: {
            doSomething: function() {
              return service.doSomething(grid);
            }
          }
        });
      }
      
      function doSomething(grid) {
        // Feature implementation
      }
    }]);
  
  /**
   * @ngdoc directive
   * @name ui-grid-my-feature
   * @description Directive for my feature
   */
  angular.module('ui.grid.myFeature')
    .directive('uiGridMyFeature', ['uiGridMyFeatureService', function(uiGridMyFeatureService) {
      return {
        replace: true,
        priority: 0,
        require: '^uiGrid',
        scope: false,
        link: function($scope, $elm, $attrs, uiGridCtrl) {
          var grid = uiGridCtrl.grid;
          
          // Initialize the feature
          uiGridMyFeatureService.initializeGrid(grid);
          
          // Feature-specific logic
          $scope.$on('$destroy', function() {
            // Cleanup
          });
        }
      };
    }]);
})();
```

### Adding API Methods and Events

```javascript
// In your service initializeGrid function:

// Register events that other code can listen to
grid.api.registerEventsFromObject({
  myFeature: {
    rowProcessed: function(grid, row) {},
    dataChanged: function(grid) {}
  }
});

// Register methods that other code can call
grid.api.registerMethodsFromObject({
  myFeature: {
    processRow: function(row) {
      return service.processRow(grid, row);
    },
    getProcessedData: function() {
      return service.getProcessedData(grid);
    }
  }
});

// Fire events from your feature
grid.api.myFeature.raise.rowProcessed(grid, row);

// Call methods from other features
if (grid.api.selection) {
  var selectedRows = grid.api.selection.getSelectedRows();
}
```

## Custom Cell Renderers

### Basic Custom Renderer

```javascript
// In your column definition:
{
  field: 'status',
  cellTemplate: '<div class="ui-grid-cell-contents"><my-status-renderer value="row.entity.status"></my-status-renderer></div>'
}

// Custom directive:
angular.module('myApp').directive('myStatusRenderer', function() {
  return {
    restrict: 'E',
    scope: { value: '=' },
    template: '<span class="status-badge status-{{value}}">{{value | uppercase}}</span>'
  };
});
```

### Advanced Cell Renderer with Editing

```javascript
angular.module('myApp').directive('myComplexRenderer', function() {
  return {
    restrict: 'E',
    scope: {
      row: '=',
      col: '=',
      grid: '='
    },
    template: `
      <div class="complex-cell">
        <div ng-if="!editing" ng-click="startEdit()">
          <span class="display-value">{{displayValue}}</span>
          <button class="btn btn-xs btn-primary">Edit</button>
        </div>
        <div ng-if="editing">
          <input ng-model="editValue" class="form-control">
          <button ng-click="saveEdit()" class="btn btn-xs btn-success">Save</button>
          <button ng-click="cancelEdit()" class="btn btn-xs btn-default">Cancel</button>
        </div>
      </div>
    `,
    link: function(scope) {
      scope.editing = false;
      scope.displayValue = scope.row.entity[scope.col.field];
      
      scope.startEdit = function() {
        scope.editing = true;
        scope.editValue = scope.displayValue;
      };
      
      scope.saveEdit = function() {
        scope.row.entity[scope.col.field] = scope.editValue;
        scope.displayValue = scope.editValue;
        scope.editing = false;
        
        // Trigger grid events
        scope.grid.api.edit.raise.afterCellEdit(
          scope.row.entity, 
          scope.col, 
          scope.editValue, 
          scope.displayValue
        );
      };
      
      scope.cancelEdit = function() {
        scope.editing = false;
        scope.editValue = scope.displayValue;
      };
    }
  };
});
```

## Custom Filters

### Creating Custom Filter Conditions

```javascript
angular.module('myApp').config(['uiGridConstants', function(uiGridConstants) {
  // Add custom filter condition
  uiGridConstants.filter.MY_CUSTOM_FILTER = 199;
}]);

angular.module('myApp').service('myCustomFilterService', function() {
  return {
    condition: function(searchTerm, cellValue, row, column) {
      // Custom filter logic
      if (searchTerm === 'special') {
        return cellValue.indexOf('special') !== -1;
      }
      return true;
    }
  };
});

// In column definition:
{
  field: 'description',
  filter: {
    condition: myCustomFilterService.condition,
    placeholder: 'Enter special filter...'
  }
}
```

### Multi-Value Select Filter

```javascript
angular.module('myApp').directive('multiSelectFilter', function() {
  return {
    restrict: 'E',
    scope: {
      column: '='
    },
    template: `
      <div class="multi-select-filter">
        <label ng-repeat="option in options">
          <input type="checkbox" ng-model="option.selected" ng-change="filterChanged()">
          {{option.label}}
        </label>
      </div>
    `,
    link: function(scope) {
      scope.options = [
        { value: 'active', label: 'Active', selected: false },
        { value: 'inactive', label: 'Inactive', selected: false },
        { value: 'pending', label: 'Pending', selected: false }
      ];
      
      scope.filterChanged = function() {
        var selectedValues = scope.options
          .filter(function(opt) { return opt.selected; })
          .map(function(opt) { return opt.value; });
          
        if (selectedValues.length === 0) {
          scope.column.filters[0].term = undefined;
        } else {
          scope.column.filters[0].term = selectedValues.join(',');
          scope.column.filters[0].condition = function(searchTerm, cellValue) {
            return searchTerm.split(',').includes(cellValue);
          };
        }
      };
    }
  };
});
```

## Performance Optimization

### Virtualization Customization

```javascript
// Custom virtualization settings
$scope.gridOptions = {
  // Customize virtualization behavior
  virtualizationThreshold: 50,
  scrollThreshold: 4,
  scrollDebounce: 300,
  wheelScrollThrottle: 70,
  
  // Custom row height calculation
  rowHeight: function(row) {
    if (row.entity.isExpanded) {
      return 60; // Expanded row height
    }
    return 30; // Normal row height
  },
  
  // Optimize change detection
  rowIdentity: function(row) {
    return row.id; // Use stable identifier
  }
};
```

### Memory Management

```javascript
// Proper cleanup in custom features
angular.module('myApp').service('myFeatureService', function() {
  var cleanupFunctions = [];
  
  return {
    initializeGrid: function(grid) {
      // Store cleanup functions
      cleanupFunctions.push(function() {
        // Feature-specific cleanup
      });
      
      // Listen for grid destruction
      grid.api.core.on.gridDestroy(null, function() {
        cleanup();
      });
    }
  };
  
  function cleanup() {
    cleanupFunctions.forEach(function(fn) {
      fn();
    });
    cleanupFunctions.length = 0;
  }
});
```

## Testing Custom Features

### Unit Testing

```javascript
describe('MyFeatureService', function() {
  var service, grid, $q, $rootScope;
  
  beforeEach(function() {
    module('ui.grid');
    module('ui.grid.myFeature');
    
    inject(function(_myFeatureService_, _$q_, _$rootScope_) {
      service = _myFeatureService_;
      $q = _$q_;
      $rootScope = _$rootScope_;
    });
    
    grid = {
      options: {},
      api: {
        registerEventsFromObject: jasmine.createSpy(),
        registerMethodsFromObject: jasmine.createSpy()
      }
    };
  });
  
  it('should initialize grid correctly', function() {
    service.initializeGrid(grid);
    
    expect(grid.api.registerEventsFromObject).toHaveBeenCalled();
    expect(grid.api.registerMethodsFromObject).toHaveBeenCalled();
  });
  
  it('should process data correctly', function() {
    var testData = [{ id: 1, name: 'Test' }];
    var result = service.processData(testData);
    
    expect(result).toBeDefined();
    expect(result.length).toBe(1);
  });
});
```

### Integration Testing

```javascript
describe('MyFeature Integration', function() {
  var element, scope, gridApi;
  
  beforeEach(function() {
    module('ui.grid');
    module('ui.grid.myFeature');
    
    inject(function($compile, $rootScope) {
      scope = $rootScope.$new();
      scope.gridOptions = {
        data: [{ id: 1, name: 'Test' }],
        enableMyFeature: true
      };
      
      element = angular.element(
        '<div ui-grid="gridOptions" ui-grid-my-feature></div>'
      );
      
      $compile(element)(scope);
      scope.$digest();
      
      gridApi = scope.gridOptions.api;
    });
  });
  
  it('should add feature API methods', function() {
    expect(gridApi.myFeature.doSomething).toBeDefined();
  });
  
  it('should trigger feature events', function() {
    var eventFired = false;
    gridApi.myFeature.on.featureEvent(scope, function() {
      eventFired = true;
    });
    
    // Trigger the event
    gridApi.myFeature.raise.featureEvent();
    
    expect(eventFired).toBe(true);
  });
});
```

## Debugging and Profiling

### Debug Mode

```javascript
// Enable debug logging
$scope.gridOptions = {
  debug: {
    verbose: true // Enables detailed logging
  }
};

// Custom debug logging in your features
if (grid.options.debug && grid.options.debug.verbose) {
  console.log('MyFeature: Processing data', data);
}
```

### Performance Profiling

```javascript
// Profile custom operations
function profiledFunction(grid, data) {
  var start = performance.now();
  
  // Your custom logic here
  processData(data);
  
  var end = performance.now();
  console.log('ProcessData took ' + (end - start) + ' milliseconds');
}

// Profile rendering
grid.api.core.on.rowsRendered(null, function() {
  console.log('Rows rendered at:', new Date().toISOString());
});
```

### Memory Profiling

```javascript
// Monitor memory usage
function checkMemoryUsage() {
  if (performance.memory) {
    console.log('Memory usage:', {
      used: performance.memory.usedJSHeapSize,
      total: performance.memory.totalJSHeapSize,
      limit: performance.memory.jsHeapSizeLimit
    });
  }
}

// Check periodically
setInterval(checkMemoryUsage, 5000);
```

## Build and Development Tools

### Development Setup

```bash
# Clone the repository
git clone https://github.com/angular-ui/ng-grid.git
cd ng-grid

# Install dependencies
npm install

# Build the project
grunt

# Watch for changes
grunt watch

# Run tests
grunt test

# Start development server
grunt serve
```

### Custom Build Configuration

```javascript
// Gruntfile.js modifications for custom builds
module.exports = function(grunt) {
  grunt.initConfig({
    // Custom build targets
    concat: {
      myCustomBuild: {
        src: [
          'src/js/core/**/*.js',
          'src/js/features/selection/**/*.js',
          'src/js/features/edit/**/*.js',
          'custom/my-feature/**/*.js'
        ],
        dest: 'dist/ui-grid.custom.js'
      }
    }
  });
};
```

This developer guide provides the foundation for extending UI-Grid with custom features, optimizing performance, and maintaining code quality.