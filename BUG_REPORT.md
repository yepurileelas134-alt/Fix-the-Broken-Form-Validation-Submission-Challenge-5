# Bug Report - TrackFlow Bug Form

## Bug #1: Empty Submissions Accepted
- **Symptom**: The form allows submission even when required fields like Title, Severity, and Description are empty.
- **Root Cause**: The `validate()` function is a stub that always returns `true`, and its return value isn't checked in `handleSubmit` to gate the API call.

## Bug #2: No Loading State (Double Submissions)
- **Symptom**: The Submit button remains active during the 1.8s API call delay. Users can click it multiple times, leading to duplicate bug reports.
- **Root Cause**: `setLoading(true)` is never called before the `await submitBugReport(form)` call, and the button's `disabled` property is not wired to any loading state.

## Bug #3: Form Not Cleared After Success
- **Symptom**: After a successful submission and receiving a confirmation banner, the form fields still contain the previous data.
- **Root Cause**: There is no logic to reset the `form` state back to empty values in the `try` block after a successful API response.

## Bug #4: Server-Side Errors Swallowed
- **Symptom**: When the API returns an error (e.g., when the title contains "login"), the error is caught but no feedback is shown to the user.
- **Root Cause**: The `catch` block in `handleSubmit` is empty, and the `serverError` state is never updated to inform the user of the failure.

## Bug #5: No per-field Validation Messages
- **Symptom**: Even if validation fails, the user doesn't see which specific field is problematic or why.
- **Root Cause**: The `errors` state is never populated by the `validate()` function, and there are no conditional rendering blocks in the JSX to display error messages below the inputs.

## Bug #6: Invalid Steps Count Accepted
- **Symptom**: The "No. of Steps" field accepts zero or negative numbers, which are invalid for a bug report.
- **Root Cause**: There is no validation logic in `validate()` to check if `stepsCount` is a positive integer greater than zero.
