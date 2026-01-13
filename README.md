# Task Glitch – Bug Fixes Progress

This repository contains the Task Glitch app, a sales task management tool.  
Below is a checklist of bugs and their fix status:

### Bug Fixes

- [x] **Bug 1 – Double Fetch Issue**  
  Fixed duplicate data loading by removing an unintended second `useEffect` that fetched and appended tasks. Ensured a single initialization fetch with an empty dependency array and safe state replacement.  
  **Files:** `useTasks.ts`

- [x] **Bug 2 – Undo Snackbar Logic**  
  Fixed the "last deleted" state not clearing correctly when the snackbar closes. Now `undoDelete` works only within the snackbar window.  
  **Files:** `App.tsx`, `TasksContext.tsx`

- [x] **Bug 3 – Unstable Sorting**  
  Added a deterministic tie-breaker (task title) for ROI and priority ties. Ensures stable sorting and prevents flickering.  
  **Files:** `logic.ts`

- [x] **Bug 4 – Double Dialog Opening**  
  Stopped event bubbling in Edit/Delete buttons so only the intended dialog opens.  
  **Files:** `TaskTable.tsx`

- [x] **Bug 5 – ROI Calculation & Validation**  
  Added safe ROI calculations to handle `timeTaken = 0`, missing revenue, and invalid inputs. ROI now displays properly.  
  **Files:** `logic.ts`

**Trace the logic to its source**

Every bug affects a part of the code. You need to find the file responsible.

**Example:**

**Bug 1 – Double Fetch**

Task loading usually happens in a hook or context.
Look at useTasks.ts or TasksContext.tsx.
You’ll see a useEffect that fetches tasks from JSON or API.
If there’s more than one useEffect fetching data, that’s the cause.
Fix: Remove duplicates → ✅ File: useTasks.ts.

**Bug 2 – Undo Snackbar**

Undo logic depends on lastDeleted state and Snackbar.
Check where you manage task deletion → useTasks.ts or TasksContext.tsx.
Check where Snackbar closes → App.tsx.
Fix: Clear lastDeleted on close → ✅ Files: App.tsx, TasksContext.tsx.

**Bug 3 – Unstable Sorting**

Sorting is in a utility file, often called logic.ts.
The flicker happens because multiple tasks have the same ROI/priority.
Fix: Add tie-breaker (title, ID) in sorting function → ✅ File: logic.ts.

**Bug 4 – Double Dialog Opening**

Edit/Delete buttons inside TaskTable.tsx.
Problem: click events bubble to row click → both dialogs open.
Fix: Add e.stopPropagation() in button handlers → ✅ File: TaskTable.tsx.

**Bug 5 – ROI Calculation**

ROI = Revenue ÷ Time Taken.
Divide by zero or invalid input causes Infinity/NaN.
Look for ROI calculation function → logic.ts.
Fix: Add checks → ✅ File: logic.ts.
