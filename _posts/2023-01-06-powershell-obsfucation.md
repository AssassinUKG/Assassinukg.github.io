---
title: Ports 
date: 2022-12-04 16:35:00 +00:00
categories: [ports]
tags: [ports]     # TAG names should always be lowercase
---


# Powershell Obsfucation tricks with examples

Here is a list of obsfucation techniques. 

- String encoding: This involves converting strings into a different format, such as base64 or hexadecimal, to make them harder to read and understand.
- Command substitution: This involves using commands that generate output and assigning the output to a variable, making it difficult to determine the original command.
- Variable renaming: This involves giving variables intentionally confusing names to make it harder to understand the code.
- Function renaming: Similar to variable renaming, this involves giving functions intentionally confusing names to make the code harder to understand.
- Comment removal: Removing comments from the code can make it harder to understand and follow.
- Code concatenation: This involves breaking up code into smaller pieces and concatenating them together, making it harder to read and understand.
- Control flow obfuscation: This involves using techniques like loop unrolling, where a loop is replaced with multiple copies of the code it contains, making it harder to understand the code's logic.

Remember that while obfuscation can make it harder for someone to understand your code, it is not a foolproof method for hiding it. It is always possible for someone with enough knowledge and resources to reverse-engineer obfuscated code.


1. String encoding:

```powershell
$string = "Hello World"
$encodedString = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($string))
# Output: "SGVsbG8gV29ybGQ="
```

1. Command substitution:

```
$output = dir | Out-String
$command = "Get-ChildItem"
Invoke-Expression $command
# Output: The contents of the current directory
```

2. Variable renaming:

```
$a = 1
$b = 2
$c = 3
$xyz = $a + $b + $c
# Output: 6
```

3. Function renaming:

```
function abc {
    "Hello World"
}

abc
# Output: "Hello World"
```

4. Comment removal:

```
# This is a comment
Write-Output "Hello World"

# Output: "Hello World"
Code concatenation:
Copy code
$code = "Write-Output `"H`" + `"ello`""
$code += " `"W`" + `"orld`""
Invoke-Expression $code
# Output: "Hello World"
```

5. Control flow obfuscation:

```
$i = 0
$output = ""

while ($i -lt 5) {
    $output += "Hello "
    $i++
}

$output += "World"
Write-Output $output
# Output: "Hello Hello Hello Hello Hello World"
```
