## StringBuilder performance best practices

    In .NET, strings are immutable. Each operation that appears to modify a string object creates a new string. When you are using a loop to concatenate a random number of strings, you should use a StringBuilder to improve the performance. StringBuilder represents a mutable string of characters. So, you can modify it without allocating a new string.

1. Call Append multiple times instead of concatenating string

   - You should call `Append` with each part of the concatenation and then call `AppendLine`. Note that internally, `Append` use `ISpanFormattable` to avoid allocation while converting the number to a string.

2. Use Append(char) instead of Append(string) when possible

   - If you need to add a single character, you should use `Append(char)` instead of `Append(string)`. The former method is about 40% faster. You can also replace `AppendLine("a")` with `Append('a').AppendLine()`.

3. Use AppendFormat instead of Append(string.Format())

   - Using `ToString` is a little bit faster, but `AppendFormat` is the one that allocates the least memory.

4. Use Append(ReadOnlySpan `<char>`) instead Append(str.SubString())

   - Instead of using `string.Substring`, you can use the overload of `Append` that supports start index and length. It is similar to Substring except it doesn't need to create the intermediate string, so it allocates less memory. In .NET Core you can also use a `Span<char>` to create a substring.

5. Use AppendJoin instead of Append(string.Join())

   - The `StringBuilder.AppendJoin` method only exists in .NET Core, so some people may not know it. You should use it instead of using `string.Join`.

6. Set the capacity of the StringBuilder

   - If you know approximately the final size of the string you want to build, you should set the initial capacity. It doesn't change a lot in terms of speed, but it can reduce the number of allocations. Note that you can use `EnsureCapacity` to increase the capacity of an existing `StringBuilder`.

7. Use a pool of StringBuilder
   - If you use lots of `StringBuilder`, you may want to use a reusable pool of `StringBuilder` to avoid lots of allocations. Instead of creating a new instance of `StringBuilder` when you need it, you get an existing one from the pool. Then, you return the instance to the pool once you finish using it. It seems to be a little bit slower, but it drastically reduces the allocations. This also means that you'll spend less time in the GC.
