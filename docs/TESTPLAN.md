# Test Plan — Account Management System (COBOL)

## Overview

This test plan documents comprehensive test cases for the Account Management System COBOL application. It covers all business logic and use cases, including balance inquiries, credit transactions, debit transactions, and error handling. These test cases will be used to validate business requirements and serve as a foundation for the Node.js application migration.

---

## Test Cases

| Test Case ID | Test Case Description | Pre-conditions | Test Steps | Expected Result | Actual Result | Status | Comments |
|---|---|---|---|---|---|---|---|
| TC_001 | View Balance - Initial Balance Display | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select menu option 1 (View Balance)<br/>3. Observe output | Display "Current balance: 1000.00" | | | |
| TC_002 | Credit Account - Single Credit Transaction | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select menu option 2 (Credit Account)<br/>3. Enter amount: 500.00<br/>4. Observe balance update | Display "Amount credited. New balance: 1500.00" | | | |
| TC_003 | Credit Account - Multiple Sequential Credits | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Credit 300.00 (balance = 1300.00)<br/>3. Select option 2 again<br/>4. Credit 200.00<br/>5. View balance | Display final balance of 1500.00 | | | Validates balance persistence across transactions |
| TC_004 | Debit Account - Successful Debit | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select menu option 3 (Debit Account)<br/>3. Enter amount: 250.00<br/>4. Observe balance update | Display "Amount debited. New balance: 750.00" | | | |
| TC_005 | Debit Account - Multiple Sequential Debits | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Debit 300.00 (balance = 700.00)<br/>3. Select option 3 again<br/>4. Debit 200.00<br/>5. View balance | Display final balance of 500.00 | | | Validates balance persistence across debits |
| TC_006 | Debit Account - Insufficient Funds (Debit Rejected) | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select menu option 3 (Debit Account)<br/>3. Enter amount: 1500.00 (exceeds balance)<br/>4. Observe output | Display "Insufficient funds for this debit." and balance remains 1000.00 | | | |
| TC_007 | Debit Account - Exact Match (Debit All Funds) | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select menu option 3 (Debit Account)<br/>3. Enter amount: 1000.00 (equals balance)<br/>4. View balance | Display "Amount debited. New balance: 0.00" | | | Edge case: boundary condition |
| TC_008 | Debit Account - Insufficient Funds Edge Case | Application started with balance of 500.00 | 1. Run application<br/>2. Select menu option 3 (Debit Account)<br/>3. Enter amount: 500.01 (exceeds by 0.01)<br/>4. Observe output | Display "Insufficient funds for this debit." and balance remains 500.00 | | | Tests decimal precision handling |
| TC_009 | Menu Navigation - Valid Choice 1 | Application started | 1. Run application<br/>2. Select menu option 1<br/>3. Observe response | Menu option 1 executes View Balance successfully | | | |
| TC_010 | Menu Navigation - Valid Choice 2 | Application started | 1. Run application<br/>2. Select menu option 2<br/>3. Enter any valid amount<br/>4. Observe response | Menu option 2 executes Credit operation successfully | | | |
| TC_011 | Menu Navigation - Valid Choice 3 | Application started | 1. Run application<br/>2. Select menu option 3<br/>3. Enter any valid amount<br/>4. Observe response | Menu option 3 executes Debit operation successfully | | | |
| TC_012 | Menu Navigation - Valid Choice 4 (Exit) | Application started | 1. Run application<br/>2. Select menu option 4<br/>3. Observe output | Display "Exiting the program. Goodbye!" and application terminates | | | |
| TC_013 | Menu Navigation - Invalid Choice (0) | Application started; menu displayed | 1. Run application<br/>2. Enter 0 at the menu prompt<br/>3. Observe output | Display "Invalid choice, please select 1-4." and redisplay menu | | | |
| TC_014 | Menu Navigation - Invalid Choice (5) | Application started; menu displayed | 1. Run application<br/>2. Enter 5 at the menu prompt<br/>3. Observe output | Display "Invalid choice, please select 1-4." and redisplay menu | | | |
| TC_015 | Menu Navigation - Invalid Choice (Negative) | Application started; menu displayed | 1. Run application<br/>2. Enter -1 at the menu prompt<br/>3. Observe output | Display "Invalid choice, please select 1-4." and redisplay menu | | | |
| TC_016 | Complex Transaction Sequence - Mixed Operations | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Credit 300.00 (balance = 1300.00)<br/>3. View balance<br/>4. Debit 200.00 (balance = 1100.00)<br/>5. View balance<br/>6. Credit 150.00 (balance = 1250.00)<br/>7. Final view balance | Final balance displays as 1250.00 | | | Tests transaction sequence and data consistency |
| TC_017 | Balance Persistence After Credit and Debit | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Credit 500.00 (balance = 1500.00)<br/>3. Debit 750.00 (balance = 750.00)<br/>4. Exit and restart application<br/>5. View balance | Initial balance resets to 1000.00 (no persistence to disk) | | | Validates in-memory storage only; no file persistence |
| TC_018 | Data Access Layer - READ Operation | Application started | 1. Run "View Balance" operation<br/>2. Trigger internal READ from DataProgram<br/>3. Verify balance is retrieved correctly | DataProgram successfully returns STORAGE-BALANCE to Operations | | | Tests data layer READ functionality |
| TC_019 | Data Access Layer - WRITE Operation | Application started; initial balance is 1000.00 | 1. Run "Credit Account" with amount 300.00<br/>2. Trigger internal WRITE to DataProgram<br/>3. Verify balance is updated correctly | DataProgram successfully updates STORAGE-BALANCE to 1300.00 | | | Tests data layer WRITE functionality |
| TC_020 | Data Validation - Decimal Precision | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Credit 100.50 (balance = 1100.50)<br/>3. Debit 50.25 (balance = 1050.25)<br/>4. View balance | Balance displays with correct 2 decimal places: 1050.25 | | | Validates PIC 9(6)V99 format handling |
| TC_021 | Boundary Test - Maximum Balance | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Credit 999000.00 (would result in 999999.99 max)<br/>3. View balance<br/>4. Attempt to credit additional amount | Balance stored as 9(6)V99; verify max value handling (999999.99) | | | Tests upper boundary of balance field |
| TC_022 | Boundary Test - Zero Balance | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Debit 1000.00<br/>3. View balance<br/>4. Attempt to debit any amount | Balance shows 0.00; new debit rejected with insufficient funds message | | | Tests zero balance state |
| TC_023 | User Input - Credit with Decimal Amount | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select Credit Account<br/>3. Enter 250.75<br/>4. Confirm balance updates correctly | Display "Amount credited. New balance: 1250.75" | | | Tests decimal input handling for credit |
| TC_024 | User Input - Debit with Decimal Amount | Application started; initial balance is 1000.00 | 1. Run application<br/>2. Select Debit Account<br/>3. Enter 100.50<br/>4. Confirm balance updates correctly | Display "Amount debited. New balance: 899.50" | | | Tests decimal input handling for debit |
| TC_025 | Menu Loop - Continuous Operation | Application started | 1. Run application<br/>2. Execute View Balance (option 1)<br/>3. Execute Credit (option 2)<br/>4. Execute Debit (option 3)<br/>5. Menu displayed again after each operation<br/>6. Confirm loop continues | After each operation, menu is redisplayed and loop continues until option 4 (Exit) is selected | | | Tests PERFORM UNTIL loop logic |

---

## Test Coverage Summary

### Functional Requirements Covered
- ✅ View current account balance
- ✅ Credit funds to account
- ✅ Debit funds from account (with overdraft protection)
- ✅ Interactive menu navigation
- ✅ Input validation

### Business Rules Validated
- ✅ Initial balance of 1000.00
- ✅ Overdraft protection (reject debit if insufficient funds)
- ✅ Decimal precision (2 decimal places)
- ✅ In-memory data storage (no persistence)
- ✅ Menu-driven interaction with repeat capability

### Edge Cases Covered
- ✅ Exact balance debit (boundary condition)
- ✅ Debit with minor decimal overage (0.01 precision)
- ✅ Invalid menu choices
- ✅ Zero balance state
- ✅ Maximum balance boundary
- ✅ Sequential mixed operations

---

## Notes for Node.js Migration

When implementing unit and integration tests in Node.js, consider:

1. **Separation of Concerns:** Break down COBOL subroutines into separate functions/modules.
2. **State Management:** Implement proper state management (consider whether to use in-memory or add persistence).
3. **Input Validation:** Validate numeric inputs before processing.
4. **Error Handling:** Use proper error handling and messages instead of bare `IF` statements.
5. **Testing Framework:** Use Jest, Mocha, or similar for unit tests.
6. **Mocking:** Mock user input and data access layer for unit tests.
