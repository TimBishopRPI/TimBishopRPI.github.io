# CSCI 1200 Lecture 7

## O(n) Notation

`O(n)` is just a representation of the time a function would take based on the function having `n` elements
- `O(1)` is constant, it always takes the same amount of time
- `O(log n)` means that, in general, your program iterates over some, but not all, of the elements
- `O(n)` in general, program iterates over EVERY element
- `O(n log n)` a mixture between `O(n)` and `O(log n)`, usually sorting where you have to see every element at least once
- `O(n^2)` program iterates over all elements a number of times proportional to n
- `O(2^n)` is bad program design, doubles number of 

The above are listed from least impactful to most impactful.

When calculating `O(N)` notation, the highest power overrides all smaller ones. Take, for example:

```
f(n) = 2^n + n^2 + 3n + 5
```

`O(n)` would evaluate to `O(2^n)`, as it's the highest power relative to everything else in the function.

Additionally, constants in front of any of the given Big O notations should be removed. As such, something like `O(2n)` is the same as, and should be written as, `O(n)`.

-----

## Best/Worst Cases

Sometimes we have examples where the `O(n)` changes based on where data is located. Take, for example:
```c++
int loc=0;
bool found = false;
while (!found && loc < n) {
    if (x == foo[loc]) found = true;
    else loc++;
}
if (found) cout << "It is there!\n";
```

Let's check the Best, Average, and Worst cases:

- **Best Case:** The data value is right at the beginning of the array `foo`.<br>Time Complexity: `O(1)`
- **Average Case:** The data value is somewhere near the middle of the array.<br>Time Complexity: `O(log n)`
- **Worst Case:** The data value is right at the end of the array, or not present at all.<br>Time Complexity: `O(n)`
----
----
## Recursion

Recursion is the act of a function calling itself over and over until it gets to a base case. A **base case** is essentially a limiter for a recursive function: it tells the function when to stop. Take, for example, the famous Fibonacci Sequence.
- It's a function of a single integer, `n`, which is the index of the number to get from the fibonacci sequence.
- It has two base cases:
  - when n = 0, where the function returns
  - when n = 1, where the function returns 1
- Otherwise, the function says that fib(n) returns:
```
fib(n - 1) + fib(n - 2)
```

The implementation of the fibonacci function, written by me, is as follows:
```c++
size_t fibonacci(int n)
{
    if (n == 0)
        return 0;
    if (n == 1)
        return 1;
    else
        return fibonacci(n - 1) + fibonacci(n - 2);
}
```

Tying back to Big O notation, this implementation of `fibonacci()` is incredibly slow. It has a Big O notation of `O(2^n)`, which is pretty much as slow as it gets. This is different from...

## Iteration

Iteration is like recursion, but instead of going deeper in the call stack by calling itself over and over, iteration uses loops to do the same thing.

Borrowing from the CSCI1200 Lecture notes, their implementation of a recursive function to return the power of a number is as follows:

```c++
int intpow(int n, int p) {
    if (p == 0) {
        return 1;
    } else {
        return n * intpow( n, p-1 );
    }
}
```

Writing it recursively would end up something like this:

```c++
int intpow_iter(int n, int p)
{
    if (p == 0)
    {
        return 1;
    }

    int total = 1;
    for (int i = 0; i < p; i++)
    {
        total *= n;
    }

    return total;
}
```

Note the differences in what the code actually does:
- First, the code in the recursive example is noticeably shorter. This is not always the case, but it is useful to notice.
- The recursive code makes the call stack deep. What does that mean? Essentially, the call stack stores every function call currently in use, and recursing puts many, many instances of that function onto the stack.

```c++
/* Assume that intpow(int n, int p) is the same as defined above.*/

// When I call intpow(5, 3), the call stack could
//be represented kind of like this:
intpow(5, 3) returns {
    n (which is 5) * intpow(5, 2) returns {
        n (which is 5) * intpow(5, 1) returns {
            n (which is 5) * intpow(5, 0) returns {
                1
            }
        }
    }
}

// aka 5 * 5 * 5 * 1, which is 125, or 5^3.
```

## Driver/True Recursive Functions

In the CSCI 1200 notes, there is a mention of "Driver" or "True" recursive functions. This really isn't necessary, however it is interesting so I'll quickly go over it.

**Driver recursive functions** are essentially helper functions that the user can more easily fit into their programs. Usually, they are functions that just start the recursion, and return the result. The example given in the notes is:
```c++
void print_vec(std::vector<int>& v) {
    print_vec_recurse(v, 0);
}
```

The `print_vec()` function simplifies the call to the actual recursive function, `print_vec_recursive()`, by calling the recursive function at its starting point.

**True recursive functions** are the functions we've been talking about the whole time: functions that call themselves over and over again, stopping at base cases.

----

# Quickrefs: Algorithms gone over

## Fibonacci Sequence

A famous sequence defined as the following:
```
        |- 0, if n == 0
        |
fib(n) -|- 1, if n == 1
        |
        |- fib(n - 1) + fib(n - 2), else
```

Any numbers generated as a part of this sequence is called a "fibonacci number." This is all likely unneeded to be memorized, but I think it's cool.

## Binary Search

A algorithm used for searching **ALREADY SORTED** arrays for a value `x`. The idea is that, since the array is sorted, we can check the middle of the array, then depending if the value is less than, equal to, or greater than what we're looking for, look at the left side or right side of the array and repeat until we either don't find the value OR the value we land on is the value. It then returns the index of that value, if found.

In case code is more readable than a summary, this is how the CSCI department implemented it in the notes:

```c++
template <class T>
bool binsearch(const std::vector<T> &v, int low, int high, const T &x) {
    if (high == low) return x == v[low];
    int mid = (low+high) / 2;
    if (x <= v[mid])
        return binsearch(v, low, mid, x);
    else
        return binsearch(v, mid+1, high, x);
}
```

(PS, don't worry right now about what `template` means, all that matters right now is approximately how the algorithm works, so use this more as pseudo code if you have to.)