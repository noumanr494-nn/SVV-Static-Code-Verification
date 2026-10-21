# Review Checklist

**Project:** Monster Battle Game  
**File Reviewed:** `game.cpp`  
**Review Method:** Static Code Inspection

| No. | Category | Result | Observation |
|---:|---|---|---|
| 1 | Initialization | Issue | `sum` is not initialized before use in `averageAttack()`. |
| 2 | Pointers (Null/Delete) | Issue | Null pointers are not checked, and unsafe memory deletion is present. |
| 3 | Arrays / Buffers | Issue | Monster array capacity is exceeded, and the boss name buffer is unsafe. |
| 4 | Loops | Issue | `totalMonsterHealth()` uses an incorrect loop boundary. |
| 5 | Arithmetic | Issue | Unsigned damage causes incorrect health checking; damage calculation may truncate decimals. |
| 6 | Control Flow | Issue | `playerRank()` does not return a value for every possible score. |
| 7 | Function Calls | Issue | Functions may receive or use invalid pointers and invalid object states. |
| 8 | Classes | Issue | The `Game` class has unsafe copy behavior and memory-management problems. |

## Overall Result

**Status: Issues Found**

Static inspection identified defects involving memory safety, array bounds, pointer handling, initialization, arithmetic, and control flow.
