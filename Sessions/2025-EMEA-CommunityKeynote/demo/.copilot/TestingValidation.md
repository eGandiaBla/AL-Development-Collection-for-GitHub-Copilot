# Testing & Validation Guide - Data Braider

**Purpose**: Ensure quality through proper testing of new features and bug fixes.

---

## 🎯 TESTING PHILOSOPHY

**Quality is not optional.** Every code change should be tested to ensure:
- ✅ It works as intended
- ✅ It doesn't break existing functionality
- ✅ It handles errors gracefully
- ✅ It performs acceptably

---

## 🧪 TYPES OF TESTING

### 1. Unit Testing
**Purpose**: Test individual functions and procedures in isolation

**When to write**:
- Business logic calculations
- Data transformations
- Validation rules
- Utility functions

**Example**:
```al
procedure TestValidateEndpointName()
var
    ValidationResult: Boolean;
begin
    // Test valid name
    ValidationResult := ValidateEndpointName('Customer_Orders');
    Assert.IsTrue(ValidationResult, 'Valid name should pass');
    
    // Test invalid name (special characters)
    ValidationResult := ValidateEndpointName('Customer@Orders');
    Assert.IsFalse(ValidationResult, 'Invalid name should fail');
end;
```

### 2. Integration Testing
**Purpose**: Test how components work together

**When to write**:
- Data flow through multiple objects
- Event subscriber chains
- API endpoint workflows
- Configuration interactions

**Example Scenario**:
- Create endpoint configuration
- Execute data engine
- Verify JSON output format
- Check logging entries created

### 3. Manual Testing
**Purpose**: Validate user experience and edge cases

**When to perform**:
- UI workflows
- User permissions and security
- Edge cases hard to automate
- Real-world scenarios

---

## 📋 TESTING CHECKLIST

### For New Features
- [ ] **Happy path tested** - Normal use case works
- [ ] **Error conditions tested** - Invalid inputs handled
- [ ] **Edge cases tested** - Boundary conditions verified
- [ ] **Permissions tested** - Correct access controls
- [ ] **Performance tested** - Acceptable response times
- [ ] **Integration tested** - Works with existing features

### For Bug Fixes
- [ ] **Regression test created** - Reproduces the bug
- [ ] **Regression test passes** - Bug is fixed
- [ ] **Related scenarios tested** - No new bugs introduced
- [ ] **Root cause verified** - Fix addresses actual problem

---

## 🔧 TEST DATA STRATEGY

### Use CRONUS Demo Data
Business Central's standard demo company provides:
- Sample customers, vendors, items
- Posted transactions
- Standard configurations

**Tip**: Create test scenarios using CRONUS data when possible.

### Create Specific Test Data
For scenarios CRONUS doesn't cover:
- Document test data requirements in implementation plan
- Create setup procedures to generate test data
- Clean up test data after testing (when appropriate)

### Test Data Patterns

**For API Endpoints**:
```al
// Test with different record counts
TestWithNoRecords();
TestWithSingleRecord();
TestWithMultipleRecords();
TestWithLargeDataset(1000);

// Test with different data types
TestWithStringFields();
TestWithNumericFields();
TestWithDateFields();
TestWithBooleanFields();
```

**For Configuration**:
```al
// Test configuration states
TestWithDefaultConfiguration();
TestWithCustomConfiguration();
TestWithMissingConfiguration();
TestWithInvalidConfiguration();
```

---

## ⚠️ ERROR HANDLING TESTING

### Test Error Scenarios
Always test what happens when things go wrong:

**Common Error Scenarios**:
- [ ] Required field is blank
- [ ] Value exceeds maximum length
- [ ] Invalid data type (e.g., text in numeric field)
- [ ] Record doesn't exist
- [ ] User lacks permissions
- [ ] System resource unavailable

### Verify Error Messages
- [ ] Error message is clear and actionable
- [ ] Error message helps user fix the problem
- [ ] Error is logged appropriately
- [ ] System remains stable after error

---

## 📊 PERFORMANCE TESTING

### When to Performance Test
- Large data volumes (1000+ records)
- Complex calculations
- Database-intensive operations
- API endpoints (response time matters)

### Performance Targets
- **Interactive operations**: < 2 seconds
- **Batch operations**: Reasonable for volume
- **API calls**: < 5 seconds for typical request

### How to Performance Test
```al
StartTime := CurrentDateTime();
// ... perform operation ...
EndTime := CurrentDateTime();
Duration := EndTime - StartTime;

// Assert performance target
Assert.IsTrue(Duration < 2000, 'Operation took too long');
```

---

## 🔄 REGRESSION TESTING

### What is Regression Testing?
Ensuring that new changes don't break existing functionality.

### When to Regression Test
- After implementing new features
- After fixing bugs
- Before releasing updates

### Regression Test Strategy
1. **Identify affected areas** - What existing features could be impacted?
2. **Test related workflows** - Verify they still work
3. **Check integration points** - Ensure connections still function
4. **Review automated tests** - Run existing test suite

---

## ✅ TEST VALIDATION CRITERIA

### Definition of "Tested"
A feature or fix is considered tested when:

✅ **All test cases pass** - No failing tests  
✅ **Coverage is adequate** - Key scenarios covered  
✅ **Errors handled** - Error conditions tested  
✅ **Documentation updated** - Test results documented  
✅ **Reviewer verified** - Someone else validated the testing

### When Tests Fail
1. **Don't ignore failures** - Investigate and fix
2. **Update tests if requirements changed** - Keep tests aligned
3. **Document known issues** - If fixing later, track it
4. **Don't ship failing tests** - Fix or remove before release

---

## 📚 TESTING DOCUMENTATION

### In Implementation Plans
Document testing approach:
- Test scenarios to cover
- Test data needed
- Expected results
- Testing checklist

### In Code Comments
Document test purpose:
```al
// Test: Verify endpoint returns correct JSON structure
// Given: Customer_Orders endpoint configured
// When: API is called with valid parameters
// Then: Returns JSON array with required fields
```

### In Work Items
Update work item with testing notes:
- Tests written
- Test results
- Issues found during testing
- Sign-off on testing complete

---

## 🎯 QUALITY GATES

### Before Moving to "Done"
- [ ] All tests written and passing
- [ ] Error scenarios tested
- [ ] Performance verified (if applicable)
- [ ] Regression testing completed
- [ ] Testing documented

### Before Production Release
- [ ] Full test suite passes
- [ ] Manual testing in staging environment
- [ ] Performance benchmarks met
- [ ] Security testing completed (if security-related change)

---

## 💡 TESTING BEST PRACTICES

### DO:
✅ **Test early and often** - Don't wait until the end  
✅ **Write tests as you code** - TDD or alongside implementation  
✅ **Test the unhappy path** - Errors are where bugs hide  
✅ **Use realistic data** - Real-world scenarios matter  
✅ **Keep tests simple** - One concept per test  
✅ **Document test purpose** - Future you will thank you

### DON'T:
❌ **Skip testing "simple" changes** - Simple bugs exist  
❌ **Only test the happy path** - Errors happen  
❌ **Write tests that always pass** - They should fail when code breaks  
❌ **Test everything manually** - Automate what you can  
❌ **Ignore flaky tests** - Fix or remove them

---

## 🔗 REFERENCES

- **AL Testing Documentation**: https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-testing-application
- **Test Codeunits**: `SBIDataBraider_Test/src/` folder
- **Implementation Plan Templates**: `.aidocs/templates/`

---

**Remember**: Testing is not about perfection; it's about confidence. Good testing gives you confidence that your code works and won't break things.
