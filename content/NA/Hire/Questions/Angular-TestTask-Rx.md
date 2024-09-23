## Goal:
check experience how candidate can kickstart new project (project structure, tooling etc), check knowledge of different RX Operators.
## Task
Please build a signup form, with fields below:
- email
- password
- confirm password
- signup button
- reset button

Where:
email: is a required field, validated to have a value, to pass regexp.
password: is a required file, minimum length 6
confirm password: should be equal to password
signup button: enabled of form is touched and all validations above passed
reset button: disabled if nothing to reset, if form has some data - resets it to clear state.

If some validation is not passed, we need to make a highlight with error description near field with an error. For network errors we can show some common error fileld in a bottom of the form.
