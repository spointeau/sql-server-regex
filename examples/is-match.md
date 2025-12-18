# IsMatch

![SQL Regex Logo](/images/sql-regex-logo.png)

RegexIsMatch() is a scalar function that lets you test whether a regular expression matches a string. It is a SQL CLR function that exposes the [System.Text.RegularExpressions](https://msdn.microsoft.com/en-us/library/system.text.regularexpressions(v=vs.110).aspx)' [IsMatch()](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex.ismatch?view=net-10.0) method.

It returns:

- **1** if the pattern matches
- **0** otherwise

The function is intended for **validation**, not extraction.

Let's look at a few examples, inspired by a handy [Regular Expressions tutorial](http://www.regular-expressions.info/examples.html)

---

## Examples

### Digits

Check whether a string contains at least one digit.

```sql
select dbo.RegexIsMatch('ABC123', '\d+');  -- 1
select dbo.RegexIsMatch('ABC',    '\d+');  -- 0
```

---

### Letters only (whole string)

Validate that the entire string contains only letters.

```sql
select dbo.RegexIsMatch('Hello', '^[A-Za-z]+$');  -- 1
select dbo.RegexIsMatch('Hi!',   '^[A-Za-z]+$');  -- 0
```

---

### Empty or exactly two characters

```sql
select dbo.RegexIsMatch('',   '^(.{2})?$');  -- 1
select dbo.RegexIsMatch('AB', '^(.{2})?$');  -- 1
select dbo.RegexIsMatch('A',  '^(.{2})?$');  -- 0
```

---

### No underscore allowed

Validate that a string does not contain `_`.

```sql
select dbo.RegexIsMatch('abc',  '^[^_]+$');  -- 1
select dbo.RegexIsMatch('abc_', '^[^_]+$');  -- 0
```

---

### Whitespace only

Check if a string is empty or contains only whitespace.

```sql
select dbo.RegexIsMatch('',    '^\s*$');  -- 1
select dbo.RegexIsMatch('   ', '^\s*$');  -- 1
select dbo.RegexIsMatch('abc', '^\s*$');  -- 0
```

---

### Partial match vs full-string match

`RegexIsMatch()` succeeds if the pattern matches **anywhere** in the string.

```sql
select dbo.RegexIsMatch('abc_', '[^_]+');  -- 1 (matches "abc")
```

To validate the **entire** string, use anchors:

```sql
select dbo.RegexIsMatch('abc_', '^[^_]+$'); -- 0
select dbo.RegexIsMatch('abc',  '^[^_]+$'); -- 1
```

---

## Notes

- Use `^` and `$` when validating full values.
- The function uses the .NET regular expression engine.
- For extracting matched text, use `RegexMatch()` instead.