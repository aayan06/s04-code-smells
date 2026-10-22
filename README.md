# s04-code-smells
Code smells across languages

## Group Members
- Alexander Flores
- Aayan Abdullah
- Umayr Rai

## Chosen Code Smells
1. **Mysterious Name**: a function, variable or class has a name that doesn't say what it does.
2. **Duplicated Code**: the same code structure appears in more than one place, so every change has to be repeated.
3. **Loops**: a for/while loop does filtering, transforming, and accumulating all at once so the reader has to read the whole body to understand what it does.

## 1. Mysterious Name

A mysterious name is when a variable or function is named something like `x`, `tmp`, or `calc` so you can't tell what it does without reading the whole thing. Fowler says this is the first smell to fix because good names make code readable on their own.

**Why it's bad:** you waste time figuring out what the code means, and short names get mixed up easily.

**Fix:** Change Function Declaration (p. 124), Rename Variable (p. 137), or Rename Field (p. 244).

**Example:** a function called calc(d, r) could mean anything, but order_total(unit_price, quantity) tells you exactly what it does without reading the body. The same idea works in any language like Python, JavaScript, or Java. The fix is just renaming x, tmp, or proc to what the thing actually is.

**Things I found interesting in the reading:**
- Fowler and Beck say naming is one of the two hard things in programming, and that renaming is probably the refactoring they do *most often*.
- People avoid renaming because it feels like it's not worth the effort, but a good name can save hours of confusion later on.
- The coolest point: if you can't come up with a good name for something, that usually means the design itself is confusing. So struggling with a name is a hint to simplify the code, not just pick a better word.
