Using the `rbegin()` function, which returns a reverse iterator pointing to the last element in the container.

When you use:

`return *formatting.rbegin();`

You're getting a reference to the last element in the `formatting` container, which is what you just added with the `emplace_back()` function.

On the other hand, `formatting.end()` returns an iterator pointing one past the last element of the container, not the last element itself. Dereferencing this iterator would be undefined behavior.

That's why using `*formatting.rbegin()` is the correct and safe way to get a reference to the last element in the container, whereas `*formatting.end()` would be incorrect and could lead to unpredictable behavior.