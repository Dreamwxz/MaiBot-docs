# Developer Standards

## Code Standards

> If you don't have much development experience but want to avoid writing messy code, we recommend reading ["A Philosophy of Software Design"](https://cactus-proj.github.io/A-Philosophy-of-Software-Design-zh/)
> 
> We recommend this book because: many existing code issues are examples of anti-patterns discussed in this book, and the book also explores solutions to these problems.

### Class Definitions and Naming
1. All class names should use **PascalCase** (UpperCamelCase).
2. All **class attributes (constants, variables)** must be **defined at the class level**.
3. All class instance attributes must be declared and initialized in the `__init__` method.
4. All classes and functions should use triple quotes (`"""`) for documentation comments whenever possible.

### Variables and Constants
1. Constants should use **ALL_UPPERCASE** with **underscores** for naming, and type declarations should be added when possible.
2. Variables should use **all_lowercase** with **underscores** for naming, and **type declarations are mandatory**.

### Async Functions and Tasks
1. For periodic scheduled tasks that exist outside class lifecycles, inherit from the `AsyncTask base class` to implement scheduled task classes, and use `async_task_manager.add_task()` to let the async task manager handle them uniformly.
2. For async tasks within class lifecycles, use `asyncio.create_task()` to create coroutines for handling, handle exceptions appropriately, and use `Task.cancel()` and `await` to ensure tasks complete and exit properly.
3. For serial async tasks in business logic, use `await` to create coroutines for handling and block business logic until the task completes.

### Import Standards
First, we define a "package": consider every folder that directly contains Python code files as a package.
1. All `import` statements should be placed at the top of the file, unless you need to dynamically import a module or use `try import`.
2. For each package, imports between files within the same package should use relative path imports, such as `import .config` or `from .config import Config`.
3. For cross-package file imports, use absolute path imports, such as `from package.module import Class`.
4. Use Ruff to check for unused `import` statements. If any are found, delete those `import` statements.

## Developer Standards

### Reviewing PRs

We recommend referring to ["Playing with Code Review on GitHub Open Source Projects"](https://www.cnblogs.com/daniel-hutao/p/code_review.html)

### Submitting Code

Please use `ruff check` locally for checking, and **only submit after passing the check**.

For smaller modifications, you can directly push code using the direct push method.

For major modifications, please use fork, make modifications in the fork, submit a PR, and wait for review before merging.