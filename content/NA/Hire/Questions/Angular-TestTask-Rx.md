## Goal:
check experience how candidate can kickstart new project (project structure, tooling etc), check knowledge of different RX Operators.
## Task
Please build a signup form, with fields below:
- email text input
- password text input
- confirm password text input
- signup button
- reset button

Where:
- email: is a required field, validated to have a value, to pass regexp.
- password: is a required file, minimum length 6
- confirm password: should be equal to password
- signup button: enabled of form is touched, and all validations above passed
- reset button: disabled if nothing to reset, if form has some data — resets it to clear state.

If some validation is not passed, we need to make a highlight with error description near field with an error. For network and other common errors we can show some common error field in a bottom of the form.

There is no need to use any styling, if you want, you can add some [picocss](https://picocss.com/), but anyway - styling is not a goal of that task.

### Gotchas
The form should be done as a one, big Rx stream. Like [fromEvent](https://rxjs.dev/api/index/function/fromEvent) , combine events from a few inputs, and then combine them into a new stream, which will go further if all conditions passed, and in the end we should add side effect that will enable signup button.
And in a case when all works—send data to server on signup button click.

If you don't know how to implement some tasks using RxJS, implement them without Rx. In the end we still need working form with completed business logic and well grained structure of the project.

