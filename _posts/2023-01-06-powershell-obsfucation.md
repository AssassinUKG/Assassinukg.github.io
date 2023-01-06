---
title: Ports 
date: 2023-01-06 16:35:00 +00:00
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

2. Command substitution:

```powershell
$output = dir | Out-String
$command = "Get-ChildItem"
Invoke-Expression $command
# Output: The contents of the current directory
```

3. Variable renaming:

```powershell
$a = 1
$b = 2
$c = 3
$xyz = $a + $b + $c
# Output: 6
```

4. Function renaming:

```powershell
function abc {
    "Hello World"
}

abc
# Output: "Hello World"
```

5. Comment removal:

```powershell
# This is a comment
Write-Output "Hello World"

# Output: "Hello World"
```

6. Code concatenation:

```
$code = "Write-Output `"H`" + `"ello`""
$code += " `"W`" + `"orld`""
Invoke-Expression $code
# Output: "Hello World"
```

7. Control flow obfuscation:

```powershell
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

8. Charater substitution

```powershell
$string = "He110 W0r1d"
$charSub = $string.Replace("1", "l").Replace("0", "o")
# Output: "Hello World"
```

9. Integer substitution

```powershell

```

## String encoding expanded

Here is an example of encoding and decoding using base64, base32, base16 (hexadecimal), base8 (octal), and base2 (binary):

All the base's

```powershell
# Encode
$string = "Hello World"
$base64 = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($string))
$base32 = [System.Convert]::ToBase32String([System.Text.Encoding]::UTF8.GetBytes($string))
$base16 = [System.Convert]::ToString([System.Text.Encoding]::UTF8.GetBytes($string), 16)
$base8 = [System.Convert]::ToString([System.Text.Encoding]::UTF8.GetBytes($string), 8)
$base2 = [System.Convert]::ToString([System.Text.Encoding]::UTF8.GetBytes($string), 2)

# Output:
# base64: "SGVsbG8gV29ybGQ="
# base32: "NBSWY3DPEB3W64TMMQ======"
# base16: "48656c6c6f20576f726c64"
# base8: "15015415152074747464544"
# base2: "1001000 11100110 11001100 11001100 11001111 100000 1010111 11011111 11100110 11001100 11001111 11010000"

# Decode (this shows the base64 decode method only) 
$decodedString = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($base64))
# Output: "Hello World"
```

>Note that the FromBase64String and GetBytes methods of the System.Convert and System.Text.Encoding classes, respectively, can be used to decode the encoded strings. You can also use the ToBase64String, ToBase32String, and ToString methods to encode strings in these different bases.

