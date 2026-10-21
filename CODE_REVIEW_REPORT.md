# Code Review Report

**Project:** Monster Battle Game  
**Source File:** `game.cpp`  
**Review Type:** Static Code Verification

## 1. Identified Defects

| Line No. | Defect Type | Why It Is a Problem | Proposed Solution | Severity |
|---:|---|---|---|---|
| 41–42 | Array Out of Bounds | Adding more monsters than the array capacity can overwrite memory. | Check `monsterCount < MAX_MONSTERS` before adding a monster. | Critical |
| 47 | Assignment Instead of Comparison | `=` changes the monster ID instead of comparing it. | Replace `=` with `==`. | High |
| 54–56 | Null Pointer Dereference | If the monster is not found, `m` is null and accessing `m->health` is unsafe. | Check `if (m == nullptr)` before accessing the monster. | Critical |
| 55–56 | Unsigned Arithmetic | Negative damage can become a large unsigned value, and the health check may fail. | Use signed integers and validate damage before subtraction. | High |
| 64–65 | Loop Boundary Error | The loop uses `i <= monsterCount`, accessing one element beyond the valid range. | Change the condition to `i < monsterCount`. | High |
| 71–73 | Dangling Reference | The function returns a reference to a local string that is destroyed when the function ends. | Return `std::string` by value. | High |
| 77–80 | Uninitialized Variable | `sum` contains an indeterminate value before addition. An empty monster list also causes division by zero. | Initialize `sum = 0` and check `monsterCount` before division. | High |
| 84–86 | Use After Free | The monster is deleted before its name is accessed. | Save the name before deletion and manage the monster's removal safely. | Critical |
| 91 | Integer Division | Integer operands can truncate fractional damage results. | Use floating-point arithmetic if fractional damage is required. | Medium |
| 94–99 | Missing Return | Scores of 100 or less do not return a value from this non-void function. | Add a return statement for the remaining scores. | High |
| 102, 105 | Buffer Overflow Risk | A fixed-size character buffer can overflow when a long boss name is entered. | Use `std::string` or enforce a safe input limit. | High |
| 109 | Incorrect Memory Deallocation | Memory allocated using `new[]` is released using `delete`. | Use `delete[] name`, or preferably use `std::string` and avoid manual allocation. | High |
| 119 | Array Capacity Violation | Six monsters are spawned although the array holds only five pointers. | Increase the capacity or prevent spawning beyond the limit. | Critical |
| 121 | Memory Leak | The dynamically allocated `bonus` monster is never deleted. | Use automatic object storage or an appropriate smart pointer. | Medium |
| 130, 85–86 | Unsafe Deletion | The deleted monster remains in the array and the count is unchanged, allowing later invalid access or double deletion. | Remove the monster from the array and update the count safely. | Critical |
| 133 | Shallow Copy / Double Deletion | Copying `Game` duplicates raw pointers. Both objects may delete the same monsters. | Implement safe copy/move behavior or use suitable smart pointers. | Critical |

## 2. Additional Review Findings

- **Line 108:** `readBoss()` may try to add another monster when the array is already full.
- **Line 123:** `attackMonster(99, 20)` requests a monster ID that does not exist. The function must handle this safely.
- **Line 124:** `getMonsterLabel(2)` uses the function that returns a dangling reference; the ID lookup is also affected by the assignment defect.
- **Line 128:** `playerRank(50)` follows a path without a return value.
- **Line 130:** `defeatMonster(3)` can trigger use-after-free and leaves the array in an unsafe state.
- **Line 133:** The default copy operation is unsafe because `Game` owns dynamically allocated objects through raw pointers.

## 3. Severity Definitions

- **Critical:** Can cause serious memory corruption, invalid memory access, or program failure.
- **High:** Can cause incorrect results, undefined behavior, or unsafe memory operations.
- **Medium:** Can produce incorrect calculations or resource-management problems.
- **Low:** Minor issue with limited impact.

## 4. Conclusion

Static code inspection identified multiple defects in memory management, pointer handling, array bounds, initialization, arithmetic, and control flow. The proposed solutions describe how each defect can be corrected before relying on the program.                        
