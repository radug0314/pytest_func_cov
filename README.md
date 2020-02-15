# Overview
This plugin attempts to provide a more meaningful test coverage metric for projects using pytest. The assumption is that,
in reality, the proportion of lines of code covered by tests does not entirely reflect how
much the tests explicitly cover. An additional indicator is how many functions, out of the total functions
defined in a project, are explicitly invoked during tests. This way, a test which
only calls a higher order function will not also count as testing the functions that it invokes during its execution, unlike traditional test coverage metrics.


pytest_func_cov provides an implementation of this metric, covering functions as well as
methods, classmethods and staticmethods. A function is considered tested if it is invoked explicitly
from a test function at least once. To make this check, the second stack frame is inspected
when a discovered function is invoked.


# Command line arguments
By default, all packages in the pytest session path are loaded and monitored, except for
those named "test" or "tests". However, pytest_func_cov does provide a command line
argument through which a specific module in the current folder can be specified:

Example:
```bash
pytest --func_cov=myproject tests/
```


# Output
Example:

```
Found 10 functions and methods:
- project.functions.calculate_mean
- project.functions.calculate_statistics
- project.functions.calculate_stdev
- project.utilities.utils.some_function
- project.utilities.utils.SomeClass.__init__
- project.utilities.utils.SomeClass.a_eq_b
- project.utilities.utils.SomeClass.a_gt_b
- project.utilities.utils.SomeClass.greeting
- project.utilities.utils.SomeClass.sum
- project.utilities.utils.SomeClass.get_name

Called 5 functions and methods:
- project.functions.calculate_statistics
- project.utilities.utils.some_function
- project.utilities.utils.SomeClass.__init__
- project.utilities.utils.SomeClass.sum
- project.utilities.utils.SomeClass.get_name

There are 5 functions and methods which were not called during testing:
- project.functions.calculate_mean
- project.functions.calculate_stdev
- project.utilities.utils.SomeClass.a_eq_b
- project.utilities.utils.SomeClass.a_gt_b
- project.utilities.utils.SomeClass.greeting

Total function coverage: 50.0%
```