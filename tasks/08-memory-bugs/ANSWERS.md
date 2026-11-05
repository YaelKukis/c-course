# Task 08 — Understanding questions

Answer in your own words, a few sentences each. Write under each `Answer:` line.

## Q1. Before your fix, `log_add` wrote `values[8]` when the log was full, and most of the time nothing visibly broke. What memory did it overwrite, and why is "it seemed to work" so dangerous?

Answer:
Writing to `values[8]` overwrote adjacent static memory located right after the array, specifically the `count` or `label` variables. "It seemed to work" is dangerous because silent memory corruption does not cause an immediate crash. Instead, it subtly alters other variables, leading to unpredictable behavior or crashes much later in execution that are extremely hard to debug.

## Q2. Explain the bug in `log_label` using the words stack, lifetime and dangling pointer. Why did making the buffer `static` fix it?

Answer:
Without `static`, the `buf` array was allocated on the **stack**, so its **lifetime** ended as soon as `log_label` returned. Returning its memory address created a **dangling pointer** pointing to invalidated stack memory. Making the buffer `static` fixed it by extending its **lifetime** to the entire execution of the program in static memory, ensuring the pointer remains valid after the function returns.

## Q3. The average bug with `int16_t sum` is not a memory bug and no sanitizer reported it. How did you find it, and how did you choose the new type?

Answer:
I found the bug by checking unit tests designed for large inputs (like `test_average_of_large_values`), where adding large numbers resulted in integer overflow and incorrect average calculations. Sanitizers don't flag signed integer overflow by default unless configured. I chose `int32_t` because the maximum possible sum of 8 `int16_t` values is around 262,136, which exceeds 16-bit limits but comfortably fits inside a 32-bit signed integer.

## Q4. Why does embedded code avoid `malloc`, and why does this log use a fixed static array instead?

Answer:
Embedded systems avoid `malloc` because dynamic memory allocation causes memory fragmentation and non-deterministic execution times, which can crash low-memory microcontrollers or miss real-time deadlines. This sensor log uses a fixed static array instead to allocate memory at compile-time, guaranteeing fixed, predictable memory usage and deterministic performance without heap corruption risks.

