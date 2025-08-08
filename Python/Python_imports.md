The `import` statement in Python allows you to bring in and use code (like functions, classes, and variables) from other Python files or libraries. This way, you don’t need to rewrite code; you can simply "import" it.

Let's go through two main ways to use the `import` statement:
1. `import xxx`
    This imports an entire module or library, making everything in it accessible through a prefix (`xxx.`). A module is a file containing Python code (often `.py` files), and a library is a collection of multiple modules (such as `numpy`, `math`, `random`, etc.).

    Example:
    Imagine a Python library called `math`, which contains several mathematical functions. If you import the `math` module like this:

    ```python
    import math
    ```
    Then, to use a function from `math`, you must include math. before the function name.

    ```python
    # Using math.sqrt to calculate the square root of 16
    result = math.sqrt(16)
    print(result)  # Output: 4.0
    ```


    By using `import math`, you're telling Python, "Bring in everything from the `math` module, and I’ll use it with the `math.` prefix."

    Advantages:
    Clearer Code: You can immediately tell which module a function or variable comes from.

    Less Conflict: Using `math.sqrt` is specific, so if another module also has a `sqrt` function, there's no confusion.

2. `from xxx import yyy`
    This imports only a specific function, class, or variable from a module, allowing you to use it directly by name without needing a prefix.

    Example:
    Using the same math module, if you only need the `sqrt` function, you can write:
    ```python
    from math import sqrt
    ``` 

    Now, you can call `sqrt` directly without the `math.` prefix:

    ```python
    # Directly using sqrt to calculate the square root of 16
    result = sqrt(16)
    print(result)  # Output: 4.0
    ```
    Advantages:
    Less Typing: You don’t have to type `math.` every time.

    More Readable for Frequent Use: If you’re using `sqrt` often, it might be cleaner without the prefix.

    Things to Keep in Mind:
    Name Conflicts: If you import `sqrt` directly from `math`, and then another `sqrt` from a different module, Python won’t know which one to use.

    Large Modules: If you only need a few things from a large module, using `from xxx import yyy` saves memory by not loading everything.

    `import xxx as zz`
    Sometimes, you might want to use a shorter name for a module. You can use `as` to rename the module when importing.

    Example:
    ```python
    import numpy as np
    ```
    Now, you can use `np` instead of `numpy`:
    ```python
    # Using numpy's array function
    import numpy as np

    my_array = np.array([1, 2, 3])
    print(my_array)  # Output: [1 2 3]
    ```