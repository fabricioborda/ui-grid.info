# Contributing Guidelines

Thank you for your interest in contributing to UI-Grid! This guide will help you get started with contributing to the project.

## Ways to Contribute

### 1. Report Issues
- Bug reports
- Feature requests
- Documentation improvements
- Performance issues

### 2. Code Contributions
- Bug fixes
- New features
- Performance optimizations
- Test improvements

### 3. Documentation
- API documentation
- Tutorials and examples
- FAQ updates
- Translation improvements

## Getting Started

### Prerequisites
- Git
- Node.js (version 10 or higher)
- npm or Yarn
- Basic knowledge of AngularJS

### Setting Up Development Environment

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/ng-grid.git
   cd ng-grid
   ```

3. **Install dependencies:**
   ```bash
   npm install
   ```

4. **Set up upstream remote:**
   ```bash
   git remote add upstream https://github.com/angular-ui/ng-grid.git
   ```

### Building the Project

```bash
# Build distribution files
npm run build

# Build and watch for changes
npm run watch

# Run tests
npm test

# Run linter
npm run lint
```

## Development Workflow

### 1. Create a Feature Branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-number
```

### 2. Make Your Changes
- Follow the coding standards (see below)
- Add tests for new functionality
- Update documentation as needed

### 3. Test Your Changes
```bash
# Run unit tests
npm test

# Run e2e tests (if applicable)
npm run e2e

# Manual testing
npm run serve
```

### 4. Commit Your Changes
Use clear, descriptive commit messages:
```bash
git commit -m "feat: add column grouping functionality

- Add groupBy option to column definitions
- Implement group header rendering
- Add tests for grouping functionality
- Update documentation

Fixes #123"
```

### 5. Push and Create Pull Request
```bash
git push origin feature/your-feature-name
```

Then create a pull request on GitHub.

## Coding Standards

### JavaScript Style Guide

Follow these conventions for consistent code:

```javascript
// Use camelCase for variables and functions
var myVariable = 'value';
function myFunction() {}

// Use PascalCase for constructors
function GridController() {}

// Use UPPER_CASE for constants
var DEFAULT_ROW_HEIGHT = 30;

// Always use semicolons
var x = 5;

// Use single quotes for strings
var message = 'Hello, world!';

// Prefer explicit comparisons
if (value === null) { }
if (array.length > 0) { }

// Use meaningful variable names
var userCount = users.length; // Good
var uc = users.length;        // Bad
```

### AngularJS Best Practices

```javascript
// Use controllerAs syntax
function GridController() {
  var vm = this;
  vm.data = [];
  vm.selectRow = selectRow;
  
  function selectRow(row) {
    // Implementation
  }
}

// Inject dependencies using array notation
GridController.$inject = ['$scope', '$timeout'];

// Use proper module organization
angular
  .module('ui.grid.core')
  .controller('GridController', GridController);
```

### CSS Guidelines

```css
/* Use meaningful class names */
.ui-grid-header-cell {}

/* Use hyphens for multi-word classes */
.ui-grid-cell-contents {}

/* Avoid deep nesting */
.ui-grid .header .cell {} /* Good */
.ui-grid .container .wrapper .header .cell {} /* Bad */

/* Use consistent spacing */
.ui-grid-cell {
  padding: 8px;
  margin: 0;
  border: 1px solid #ddd;
}
```

## Testing Guidelines

### Unit Tests

Write tests for all new functionality:

```javascript
describe('GridController', function() {
  var ctrl, scope;

  beforeEach(function() {
    module('ui.grid');
    inject(function($controller, $rootScope) {
      scope = $rootScope.$new();
      ctrl = $controller('GridController', { $scope: scope });
    });
  });

  describe('selectRow', function() {
    it('should select a row', function() {
      var row = { id: 1, name: 'Test' };
      ctrl.selectRow(row);
      expect(ctrl.selectedRow).toBe(row);
    });
  });
});
```

### E2E Tests

For complex features, include end-to-end tests:

```javascript
describe('Grid Selection', function() {
  beforeEach(function() {
    browser.get('/examples/selection');
  });

  it('should select a row when clicked', function() {
    var firstRow = element(by.css('.ui-grid-row:first-child'));
    firstRow.click();
    
    expect(firstRow.getAttribute('class')).toContain('ui-grid-row-selected');
  });
});
```

## Documentation Standards

### API Documentation

Use JSDoc comments for all public APIs:

```javascript
/**
 * @ngdoc function
 * @name GridController#selectRow
 * @methodOf ui.grid.controller:GridController
 * @description
 * Selects a row in the grid
 * 
 * @param {Object} row The row entity to select
 * @param {boolean} [multiSelect=false] Allow multiple selections
 * @returns {Promise} Promise that resolves when selection is complete
 * 
 * @example
 * <pre>
 * gridApi.selection.selectRow(myRow, true);
 * </pre>
 */
function selectRow(row, multiSelect) {
  // Implementation
}
```

### Example Documentation

Provide complete, working examples:

```javascript
/**
 * @example
 * <doc:example module="app">
 *   <doc:source>
 *     <script>
 *       angular.module('app', ['ui.grid']).controller('MainCtrl', function($scope) {
 *         $scope.gridOptions = {
 *           data: [
 *             { name: 'John', age: 30 },
 *             { name: 'Jane', age: 25 }
 *           ]
 *         };
 *       });
 *     </script>
 *     <div ng-controller="MainCtrl">
 *       <div ui-grid="gridOptions"></div>
 *     </div>
 *   </doc:source>
 * </doc:example>
 */
```

## Pull Request Process

### Before Submitting
- [ ] Code follows style guidelines
- [ ] Tests pass locally
- [ ] New functionality includes tests
- [ ] Documentation is updated
- [ ] Commit messages are clear

### Pull Request Template

Use this template for your PR description:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Manual testing completed
- [ ] E2E tests pass (if applicable)

## Screenshots/GIFs
Include if UI changes are involved

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] Tests added/updated
```

### Review Process

1. **Automated checks** must pass (CI/CD)
2. **Code review** by maintainers
3. **Testing** in different browsers (if needed)
4. **Approval** and merge

## Feature Development

### New Features

For significant new features:

1. **Create an issue** first to discuss the feature
2. **Get feedback** from maintainers and community
3. **Create design document** if complex
4. **Implement** following the agreed approach

### Breaking Changes

For breaking changes:

1. **Discuss** the necessity and impact
2. **Provide migration guide**
3. **Update documentation** thoroughly
4. **Consider deprecation** path for major changes

## Bug Fixes

### Bug Report Analysis
1. **Reproduce** the issue locally
2. **Identify** root cause
3. **Create minimal** failing test
4. **Fix** the issue
5. **Verify** fix resolves the problem

### Regression Prevention
- Add tests that would catch the bug
- Consider edge cases
- Test in different browsers if needed

## Release Process

### Versioning

UI-Grid follows [Semantic Versioning](https://semver.org/):

- **Patch** (1.0.1): Bug fixes
- **Minor** (1.1.0): New features (backward compatible)
- **Major** (2.0.0): Breaking changes

### Changelog

All notable changes are documented in CHANGELOG.md:

```markdown
## [4.11.1] - 2021-03-15
### Fixed
- Column sorting with null values
- Memory leak in cell editing

### Added
- New column type: 'currency'

### Changed
- Improved performance for large datasets
```

## Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Focus on constructive feedback
- Assume good intentions

### Communication

- Use clear, descriptive titles for issues/PRs
- Provide context and examples
- Be patient with responses
- Help others when you can

## Getting Help

If you need help with contributing:

1. **Read the documentation** thoroughly
2. **Search existing issues** for similar problems
3. **Ask questions** in issue discussions
4. **Join community** discussions

## Recognition

Contributors are recognized in:
- README.md contributors section
- Release notes
- Community highlights

Thank you for contributing to UI-Grid! Your efforts help make this project better for everyone.