---
layout: post
title:  "PHP speedup script"
date:   2018-04-02 19:50:00 +0200
categories: coding php speedup
excerpt: "Is there a way to speed up the execution of a PHP script? You can find here a few techniques."
---

Here is some PHP script execution speed up techniques that actually make sense:

- Use `static` methods when possible, this is 4 times faster
- `echo` is faster as `print`
- Use `,` instead of `.` to **concatinate a string**
- Set the **max value** in a for-loop before the loop, not in the *if* statement. `$max = count($array);for ($i=0;$i<$max;$i++) {echo $i;}`
- `unset` vars to clear memory (especially when using **arrays**)
- Use **full paths** in includes and requires, so the server doesn’t have to resolve the paths for you
- Use `strncasecmp`, `strpbrk` and `stripos` in stead of **regex**
- It’s better to use a **select statement** then multiple `if statements` with multiple `else statements`
- Suppress **errors** with an `@` is very slow!
- Close database connections when you don’t need them anymore. `$row[‘id’]` is 7 times faster as `$row[id]`
- Incrementing a `global` var is 2 times slower as a **local** var
- Wrap your string in **single quotes** (') instead of double quotes ("). It is faster because PHP searches for vars in `"…"` and not in `'...'`
- A `PHP` script is 2 to 20 times slower as a `HTML` page. Use **HTML** pages when possible.
- PHP scripts are compiled everytime you call them. Use `PHP caching software` to gain a speed optimization of **25% to 100%**.
- `$i++` is slower as `++$i`
- Not everything has to be **OOP**. Most of the time OOP is just overhead, so use it wisely!
- Use as much of the **default functions** of PHP, you don’t have te reinvent the wheel!
- Use `include()` and `require()` instead of `include_once()` and `require_once()`, as they will search everywhere to make sure file is included only once.
- Always favor built-in functions over custom functions
- Pass **unchanged variables** to a function by `reference` rather than `value`.
