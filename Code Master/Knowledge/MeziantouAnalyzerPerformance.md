## Performance of Meziantou Analyzer

1. You should use `Array.Empty<T>()` instead of `new T[0]` to avoid an allocation.
2. Optimize struct performances using StructLayout
3. Use direct methods instead of LINQ methods
4. Add RegexOptions.ExplicitCapture

   - Using named groups clarifies what is to be captured. It also makes the regex more performant, as unnamed groups will not be captured needlessly.

5. Combine LINQ methods when possible
6. Remove duplicate `OrderBy` methods, or replace the second `OrderBy` with `ThenBy`
7. Optimize Enumerable.Count() usage

   - Replace `Count()` by a more optimized method

8. Remove useless ToString call

   - `string.ToString` is a no-op call. You should remove the call to `ToString()`.

9. Replace constant Enum.ToString with nameof

   - You should use `nameof` instead of calling `ToString` on a constant enumeration value. This is mainly for performance reason.

10. Use Where before OrderBy

    - Using `Where` clause after `OrderBy` clause requires the whole collection to be sorted and then filtered. `Where` should be called first to sort only the filtered items.

11. The default implementation of `Equals` and `GetHashCode` is not performant. You should override those methods in the type.
12. The default implementation of `Equals` and `GetHashCode` is not performant. Those methods are used by `HashSet`, `Dictionary`, and similar types. You should override them in the type or use a custom `IEqualityComparer<T>`.
13. Use `Cast` instead of `Select` to cast
14. Use indexer instead of LINQ methods
15. Use the lambda parameters instead of using a closure
16. Avoid closure by using an overload with the `factoryArgument` parameter
17. Use the Regex Source Generator when possible.
18. Use `string.Create` instead of `FormattableString` when possible.
19. For performance reasons, use the `Count` property instead of `Any()`
20. Use InvokeVoidAsync when the returned value is not used

    - Simplify the usage of `IJSRuntime` or `IJSInProcessRuntime` when the returned value is not used.

21. Use `System.OperatingSystem` to check the current OS instead of `RuntimeInformation`.
22. Prefer using [`Unwrap`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskextensions.unwrap?view=net-8.0&WT.mc_id=DT-MVP-5003978) instead of using `await` twice
23. Starting with .NET 9 and C# 13, you can use `System.Threading.Lock`. When a field or a local variable is only used inside `lock`, this rule will suggest using `System.Threading.Lock` instead of `object`.
24. Use `Order` instead of `OrderBy(x => x)` to order a collection.
25. Use ContainsKey instead of TryGetValue
