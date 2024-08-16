# `topk` Function

The `topk` function returns the **top k** largest elements from a sequence of numerical data. It can apply an aggregation function, such as min, avg, or max, etc. to the **top k** elements and return the value in the result.

## Prerequisites
Ensure that you have the following:

* An appropriate [Python version](https://www.python.org/downloads/), such as 3.5 or newer to avoid syntax errors related to type annotations.

## Syntax

```py
topk(k: int, *args) -> Union[List[float], float]`
```
### Input

| Parameters  | Value    | Description |
| -------- | ------------|-------------|
| `k`       | integer     | The number of top elements to return. |
| `*args`    |  variable   | A sequence of numerical values followed optionally by an aggregation function. 


### Output

| Parameters  | Example    | Description |
| -------- | ------------|-------------|
| `Union[T1, T2, ...]`       | `Union[int, float]`     |  The value could be either an integer `List[float]` when no aggregation function is provided or a floating-point number. Or `float`, in the case of an aggregation function. |

> **_NOTE:_**  The first argument in `*args` must be a list of numerical values, such as `(List[float]` or `List[int])`. The second argument can be an **aggregation function**, such as `avg`, `min`, `max`, or `sum`. The aggregation function is an **optional** field.     

## Solution

The `topk` function can be beneficial in various use cases where the output needs to be the `k` largest element for the given input.
Here are some common steps to use this function:

* **Input validation:** The function checks if `k` is a positive integer in the list and that the aggregation function, if provided, is callable.
* **Handling the aggregation function:** If the aggregation function is passed,such as sum, min, max, etc., it applies the function to the `topk` elements.
* **Sorting:** The sequence is then sorted in descending order.
* **Error handling:** The error handling for scenarios such as:
    * **`k` is invalid**. Example:
        ```py
        topk(0, measurements)  # Raises ValueError: `k` must be a positive integer.
        ```
    * **Non-numeric sequence:** If the sequence contains **non-numerical** elements. Example:

        ```py
        topk(3, ["a", "b", "c"])  # Raises TypeError: The sequence must be a list of numerical values.
        ```
    * **Invalid aggregation function:** If the argument is `unknown` or `invalid`. Example:
        ```py
        topk(3, 'unknown', measurements)  # Raises TypeError: The second argument must be 'avg' or a callable function.
        ```
    * **Too many arguments:** If too many arguments are passed to the function. Example:
        ```py
        topk(3, sum, measurements, 100)  # Raises TypeError: Too many arguments passed to the function.
        ```

### Code sample

```py
    # Validate `k`
    if not isinstance(k, int) or k <= 0:
        raise ValueError("`k` must be a positive integer")
    
    if len(args) == 0:
        raise TypeError("Expected at least a sequence of numerical values as input")

    # Extract arguments
    if len(args) == 1:
        sequence = args[0]
        aggregation_fn = None
    elif len(args) == 2:
        aggregation_fn = args[0]
        sequence = args[1]
    else:
        raise TypeError("Too many arguments passed to the function")

    # Validate the sequence
    if not isinstance(sequence, list) or not all(isinstance(x, (int, float)) for x in sequence):
        raise TypeError("The sequence must be a list of numerical values")
    
    # Sort and get the top `k` elements
    top_k_elements = sorted(sequence, reverse=True)[:k]
    
    if aggregation_fn:
        # Apply aggregation function
        if not callable(aggregation_fn):
            raise TypeError("The second argument must be a callable function")
        
        return aggregation_fn(top_k_elements)
    
    return top_k_elements

```
## Use cases
Here are some of the usage examples to demonstrate how the `topk` function behaves under different scenarios.

### Scenario 1: Return the top three largest elements

The `topk` function will return the top 3 largest elements from the list in descending order.

```py
measurements = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
result = topk(3, measurements)
print(result)  # Output: [9, 8, 7]
```

### Scenario 2: Compute an average of the top three largest elements 

The `topk` function computes the average of the top 3 largest elements [9, 8, 7] and returns 8.

```py
result = topk(3, 'avg', measurements)
print(result)  # Output: 8
```

### Scenario 3: Return a minimum value from the top three largest elements 

The `topk` function will return the minimum value from the top 3 largest elements using a callable aggregation function, such as `min`.

```py
result = topk(3, min, measurements)
print(result)  # Output: 7
```
### Scenario 4: Return a top value from the list 

The `topk` function will return the top one single largest element from the list.

```py
result = topk(1, measurements)
print(result)  # Output: [9]
```

### Scenario 5: Return twenty elements from the list 

Since `k=20` is larger than the number of elements in the list, the `topk` function will return all of the elements in descending order.

```py
result = topk(20, measurements)
print(result)  # Output: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```