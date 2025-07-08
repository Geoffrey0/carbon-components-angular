# Angular 19 Migration Plan for carbon-components-angular

## Executive Summary

This document outlines a comprehensive incremental migration strategy to upgrade the carbon-components-angular library from Angular 14.3.0 to Angular 19.x. The migration will be executed in 6 phases over approximately 26-39 days, ensuring minimal breaking changes while leveraging the latest Angular features and performance improvements.

### Key Objectives
- Migrate from Angular 14.3.0 to Angular 19.x incrementally
- Maintain component API compatibility where possible
- Ensure all integration tests pass for the target version
- Update build toolchain and development dependencies
- Leverage new Angular 19 features for improved performance

## Current State Assessment

### Current Environment
- **Angular Version**: 14.3.0
- **Node.js**: v16 (CI uses 18.x/20.x)
- **TypeScript**: ~4.7.2
- **Build Tool**: ng-packagr
- **Testing**: Karma + Jasmine
- **Integration Tests**: Supports Angular 14-18
- **Peer Dependencies**: ^14.0.0 || ^15.0.0 || ^16.0.0 || ^17.0.0 || ^18.0.0

### Target Environment
- **Angular Version**: 19.x
- **Node.js**: ^18.19.1 || ^20.11.1 || ^22.0.0
- **TypeScript**: >=5.5.0 <5.7.0
- **RxJS**: ^6.5.3 || ^7.4.0

## Migration Strategy Overview

### Approach: Incremental Migration
We will migrate through each major Angular version sequentially:
1. **Phase 1**: Preparation and Environment Setup
2. **Phase 2**: Angular 14 → 15 Migration
3. **Phase 3**: Angular 15 → 16 Migration
4. **Phase 4**: Angular 16 → 17 Migration
5. **Phase 5**: Angular 17 → 18 Migration
6. **Phase 6**: Angular 18 → 19 Migration
7. **Phase 7**: Post-Migration Optimization

### Risk Mitigation Strategy
- Create feature branches for each phase
- Maintain rollback points at each phase
- Comprehensive testing at each step
- Monitor bundle size and performance
- Document all breaking changes

## Phase 1: Preparation and Environment Setup (1-2 days)

### Objectives
- Update development environment
- Prepare migration infrastructure
- Update CI/CD pipeline

### Prerequisites
- Backup current codebase
- Ensure all current tests pass
- Document current bundle sizes

### Step-by-Step Instructions

#### 1.1 Update Node.js Version
```bash
# Update .nvmrc
echo "18.19.1" > .nvmrc

# Update CI workflows
# Edit .github/workflows/check-build.yml
# Edit .github/workflows/test.yml
# Edit .github/workflows/publish.yml
```

#### 1.2 Create Migration Branch
```bash
git checkout -b feature/angular-19-migration
git push -u origin feature/angular-19-migration
```

#### 1.3 Update Development Dependencies
```bash
npm install -g @angular/cli@latest
npm update npm
```

#### 1.4 Baseline Testing
```bash
npm test
npm run build
npm run lint
```

### Success Criteria
- [ ] Node.js 18.19.1+ installed and configured
- [ ] All existing tests pass
- [ ] Build completes successfully
- [ ] CI/CD pipeline updated
- [ ] Migration branch created

### Rollback Plan
- Revert .nvmrc changes
- Restore original CI configuration
- Switch back to main branch

## Phase 2: Angular 14 → 15 Migration (3-5 days)

### Objectives
- Update to Angular 15.x
- Handle standalone API stabilization
- Update TypeScript to 4.8+

### Key Changes in Angular 15
- Standalone APIs became stable
- Angular Material 15 updates
- Improved tree-shaking
- New Angular CLI features

### Step-by-Step Instructions

#### 2.1 Update Angular Dependencies
```bash
ng update @angular/core@15 @angular/cli@15
ng update @angular/cdk@15 @angular/material@15
```

#### 2.2 Update TypeScript
```bash
npm install typescript@~4.8.4 --save-dev
```

#### 2.3 Update Build Dependencies
```bash
npm install ng-packagr@15 --save-dev
npm install @angular-devkit/build-angular@15 --save-dev
```

#### 2.4 Update Peer Dependencies
Update `package.json`:
```json
{
  "peerDependencies": {
    "@angular/common": "^15.0.0 || ^16.0.0 || ^17.0.0 || ^18.0.0",
    "@angular/core": "^15.0.0 || ^16.0.0 || ^17.0.0 || ^18.0.0",
    "@angular/forms": "^15.0.0 || ^16.0.0 || ^17.0.0 || ^18.0.0"
  }
}
```

#### 2.5 Create Angular 15 Integration Test
```bash
# Create integration/ng15 if it doesn't exist
# Update integration tests
cd integration/ng15
npm install
npm run build
npm test
```

### Testing Checklist
- [ ] All unit tests pass
- [ ] Library builds successfully
- [ ] Integration tests pass for ng15
- [ ] Storybook builds and runs
- [ ] No console errors in development
- [ ] Bundle size within acceptable range

### Success Criteria
- [ ] Angular 15.x successfully installed
- [ ] All tests pass
- [ ] Build completes without errors
- [ ] Integration tests work
- [ ] No breaking changes to public API

### Rollback Plan
```bash
git reset --hard HEAD~1
npm install
```

## Phase 3: Angular 15 → 16 Migration (3-5 days)

### Objectives
- Update to Angular 16.x
- Handle required inputs feature
- Update TypeScript to 4.9+

### Key Changes in Angular 16
- Required inputs
- Standalone ng-new collection
- Improved hydration
- New control flow syntax (experimental)

### Step-by-Step Instructions

#### 3.1 Update Angular Dependencies
```bash
ng update @angular/core@16 @angular/cli@16
ng update @angular/cdk@16 @angular/material@16
```

#### 3.2 Update TypeScript
```bash
npm install typescript@~4.9.5 --save-dev
```

#### 3.3 Handle Required Inputs
Review components for required inputs and add appropriate validation:
```typescript
// Example: Update components with required inputs
@Input({ required: true }) value!: string;
```

#### 3.4 Update Integration Tests
```bash
cd integration/ng16
npm install
npm run build
npm test
```

### Testing Checklist
- [ ] All unit tests pass
- [ ] Required inputs work correctly
- [ ] Library builds successfully
- [ ] Integration tests pass for ng16
- [ ] Storybook builds and runs
- [ ] No console errors

### Success Criteria
- [ ] Angular 16.x successfully installed
- [ ] Required inputs implemented where needed
- [ ] All tests pass
- [ ] No breaking changes to public API

## Phase 4: Angular 16 → 17 Migration (5-7 days)

### Objectives
- Update to Angular 17.x
- Evaluate new control flow syntax
- Update TypeScript to 5.2+
- Handle application builder changes

### Key Changes in Angular 17
- New control flow syntax (@if, @for, @switch)
- New application builder
- Improved SSR
- New lifecycle hooks

### Step-by-Step Instructions

#### 4.1 Update Angular Dependencies
```bash
ng update @angular/core@17 @angular/cli@17
ng update @angular/cdk@17 @angular/material@17
```

#### 4.2 Update TypeScript
```bash
npm install typescript@~5.2.2 --save-dev
```

#### 4.3 Evaluate Control Flow Syntax
Review templates and consider migrating to new syntax:
```html
<!-- Old syntax -->
<div *ngIf="condition">Content</div>

<!-- New syntax (optional) -->
@if (condition) {
  <div>Content</div>
}
```

#### 4.4 Update Build Configuration
Update `angular.json` for new application builder if needed.

### Testing Checklist
- [ ] All unit tests pass
- [ ] Library builds successfully
- [ ] Integration tests pass for ng17
- [ ] Control flow syntax works (if adopted)
- [ ] Storybook builds and runs

### Success Criteria
- [ ] Angular 17.x successfully installed
- [ ] Build system updated
- [ ] All tests pass
- [ ] Performance maintained or improved

## Phase 5: Angular 17 → 18 Migration (5-7 days)

### Objectives
- Update to Angular 18.x
- Handle Material 3 changes
- Update TypeScript to 5.4+
- Implement new build features

### Key Changes in Angular 18
- Material 3 design system
- Improved hydration
- Event replay
- New experimental features

### Step-by-Step Instructions

#### 5.1 Update Angular Dependencies
```bash
ng update @angular/core@18 @angular/cli@18
ng update @angular/cdk@18 @angular/material@18
```

#### 5.2 Update TypeScript
```bash
npm install typescript@~5.4.2 --save-dev
```

#### 5.3 Handle Material 3 Changes
Review and update Material components for Material 3 compatibility.

#### 5.4 Test SSR Compatibility
Ensure components work with improved SSR features.

### Testing Checklist
- [ ] All unit tests pass
- [ ] Material 3 compatibility verified
- [ ] Library builds successfully
- [ ] Integration tests pass for ng18
- [ ] SSR features work correctly

### Success Criteria
- [ ] Angular 18.x successfully installed
- [ ] Material 3 compatibility maintained
- [ ] All tests pass
- [ ] SSR features functional

## Phase 6: Angular 18 → 19 Migration (7-10 days)

### Objectives
- Update to Angular 19.x
- Implement standalone defaults
- Update TypeScript to 5.5+
- Leverage new Angular 19 features

### Key Changes in Angular 19
- Standalone defaults to true
- Incremental hydration
- Event replay enabled by default
- New signal APIs (linkedSignal, resource)
- Enhanced reactivity

### Step-by-Step Instructions

#### 6.1 Update Angular Dependencies
```bash
ng update @angular/core@19 @angular/cli@19
ng update @angular/cdk@19 @angular/material@19
```

#### 6.2 Update TypeScript
```bash
npm install typescript@~5.5.0 --save-dev
```

#### 6.3 Handle Standalone Defaults
Run Angular schematics to update standalone metadata:
```bash
ng generate @angular/core:signals
```

#### 6.4 Update Peer Dependencies
```json
{
  "peerDependencies": {
    "@angular/common": "^19.0.0",
    "@angular/core": "^19.0.0",
    "@angular/forms": "^19.0.0"
  }
}
```

#### 6.5 Create Angular 19 Integration Test
```bash
# Update integration/ng19 (create if needed)
cd integration/ng19
npm install
npm run build
npm test
```

### Testing Checklist
- [ ] All unit tests pass
- [ ] Standalone components work correctly
- [ ] Library builds successfully
- [ ] Integration tests pass for ng19
- [ ] New signal APIs functional
- [ ] Performance optimized

### Success Criteria
- [ ] Angular 19.x successfully installed
- [ ] Standalone defaults implemented
- [ ] All tests pass
- [ ] New features accessible
- [ ] Performance maintained or improved

## Phase 7: Post-Migration Optimization (2-3 days)

### Objectives
- Clean up deprecated code
- Optimize performance
- Update documentation
- Implement Angular 19 features

### Activities
- Remove deprecated APIs
- Update documentation and examples
- Optimize bundle size
- Implement incremental hydration where beneficial
- Update Storybook stories

## Risk Assessment and Mitigation

### High Risk Items
1. **Breaking Changes in Angular Material/CDK**
   - Mitigation: Thorough testing, gradual rollout
2. **TypeScript Strict Mode Issues**
   - Mitigation: Incremental TypeScript updates, comprehensive testing
3. **Build System Changes**
   - Mitigation: Test build process at each phase

### Medium Risk Items
1. **Peer Dependency Conflicts**
   - Mitigation: Careful version management, integration testing
2. **Performance Regression**
   - Mitigation: Bundle size monitoring, performance testing

## Timeline Summary

| Phase | Duration | Key Activities |
|-------|----------|----------------|
| 1 | 1-2 days | Environment setup, preparation |
| 2 | 3-5 days | Angular 15 migration |
| 3 | 3-5 days | Angular 16 migration |
| 4 | 5-7 days | Angular 17 migration |
| 5 | 5-7 days | Angular 18 migration |
| 6 | 7-10 days | Angular 19 migration |
| 7 | 2-3 days | Optimization and cleanup |

**Total Estimated Time: 26-39 days**

## Post-Migration Validation Checklist

- [ ] All unit tests pass
- [ ] All integration tests pass
- [ ] Library builds successfully
- [ ] Storybook builds and runs
- [ ] Bundle size within acceptable limits
- [ ] Performance benchmarks met
- [ ] Documentation updated
- [ ] Breaking changes documented
- [ ] Migration guide created for consumers

## Communication Plan

1. **Pre-Migration**: Announce migration plan to stakeholders
2. **During Migration**: Weekly progress updates
3. **Post-Migration**: Release notes with breaking changes and new features
4. **Consumer Support**: Migration guide and support for library consumers

---

*This migration plan should be reviewed and approved by the development team before execution. Each phase should be completed and validated before proceeding to the next phase.*
