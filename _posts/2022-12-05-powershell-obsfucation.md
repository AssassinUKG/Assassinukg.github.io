---
title: Powershell Obsfucation tricks 
date: 2022-12-05 09:49:00 +00:00
categories: [tips, tricks]
tags: [tips]     # TAG names should always be lowercase
---

PowerShell obfuscation is the practice of making it difficult to understand or reverse engineer PowerShell code. This can be useful for a variety of reasons, such as to protect intellectual property, to prevent unauthorized access to sensitive information, or to make it more difficult for attackers to exploit vulnerabilities in the code.

There are many different techniques that can be used to obfuscate PowerShell code, and the specific method that is most effective will depend on the situation and the goals of the obfuscation. Some common techniques for obfuscating PowerShell code include:

- String concatenation: This technique involves splitting strings into multiple pieces and concatenating them at runtime, which can make it difficult to read or understand the code.
- Character replacement: This technique involves replacing characters in the code with other characters or symbols, which can make it difficult to read or understand the code.
- Code encryption: This technique involves encrypting the code, which can make it difficult to access or reverse engineer the code without the appropriate decryption key.
- Control flow obfuscation: This technique involves using complex control flow structures, such as nested loops or conditional statements, to make it difficult to understand the logical flow of the code.

It's important to note that while obfuscation techniques can make it more difficult to understand or reverse engineer PowerShell code, they are not foolproof and can be defeated by skilled attackers. In addition, using obfuscation techniques can make the code more difficult to maintain or debug, and it can introduce additional complexity and overhead to the code. As such, it's important to carefully consider the potential benefits and drawbacks of obfuscation before using it in your PowerShell code.

> **_NOTE_:** All below functions were generated with the OpenAI Chat app: https://chat.openai.com/chat

## Strings

```powershell
function Obfuscate-String {
    param (
        [Parameter(Mandatory = $true)]
        [String]$InputString
    )
    $OutputString = [String]::Empty
    $InputChars = $InputString.ToCharArray()
    foreach ($InputChar in $InputChars) {
        $OutputString += [Convert]::ToInt32($InputChar)
        $OutputString += " "
    }
    $OutputString.Trim()
}
```

## Name

```powershell
function Obfuscate-Name
{
    [CmdletBinding()]
    param
    (
        [Parameter(Mandatory=$true)]
        [string]$Name
    )

    $ObfuscatedName = ($Name[0] + $Name.Length) + $Name.Substring(1, $Name.Length-2) + ($Name[$Name.Length-1] + $Name.Length)

    return $ObfuscatedName
}
```
This script takes a string representing a name as input and returns an obfuscated version of the name.   
The obfuscated name is created by concatenating the first and last letters of the original name with the length of the name,  
and then adding the middle part of the name in between.   
For example, if the original name is "John", the obfuscated name would be "J5hno".  
This is just one example of a name obfuscator using concatenation, and there are many other ways to implement this type of script.  

```powershell
function Obfuscate-Name {
    param (
        [parameter(Mandatory=$true)]
        [string]$Name
    )
    # Replace vowels with numbers
    $Name = $Name -replace "[aeiou]", "1"
    # Reverse the string
    $Name = $Name | ForEach-Object { [char[]]$_ } | 
        ForEach-Object { $_[($_.Count - 1)..0] } |
        ForEach-Object { [string]$_ }
    # Concatenate the name with a random string
    $RandomString = [System.IO.Path]::GetRandomFileName()
    $ObfuscatedName = "$Name$RandomString"
    return $ObfuscatedName
}
```

```powershell
function Deobfuscate-Name {
    param (
        [parameter(Mandatory=$true)]
        [string]$Name
    )
    # Remove the random string from the end of the name
    $RandomStringLength = [System.IO.Path]::GetRandomFileName().Length
    $Name = $Name.Substring(0, $Name.Length - $RandomStringLength)
    # Reverse the string
    $Name = $Name | ForEach-Object { [char[]]$_ } | 
        ForEach-Object { $_[($_.Count - 1)..0] } |
        ForEach-Object { [string]$_ }
    # Replace numbers with vowels
    $Name = $Name -replace "1", "a"
    return $Name
}
```

## Variables

```powershell
function Obfuscate-Variable
{
    [CmdletBinding()]
    param (
        [Parameter(Mandatory=$true)]
        [string]$InputVariable
    )

    $OutputVariable = $InputVariable.ToCharArray() | ForEach-Object { [char]($_ -bxor 0x13) }

    return $OutputVariable
}
```

## Class Names

```powershell
function Obfuscate-ClassName {
  param (
    [string] $className
  )
  $obfuscatedClassName = ""
  foreach ($char in $className) {
    $obfuscatedClassName += [char] ([int] [char] $char + 5)
  }
  return $obfuscatedClassName
}
```


```powershell
function Obfuscate-ClassName
{
  # Get the input class name
  param (
    [string]$ClassName
  )

  # Initialize an empty string to store the obfuscated class name
  $ObfuscatedClassName = ""

  # Loop through each character in the class name
  foreach ($char in $ClassName.ToCharArray())
  {
    # Add the Unicode value of the character to the obfuscated class name
    $ObfuscatedClassName += [int]$char
  }

  # Return the obfuscated class name
  return $ObfuscatedClassName
}

# Define a test class name
$className = "TestClass"

# Obfuscate the class name
$obfuscatedClassName = Obfuscate-ClassName -ClassName $className

# Output the obfuscated class name
Write-Output $obfuscatedClassName

```

## Numbers

```powershell
function Obfuscate-Number([int]$number) {
  $random = New-Object System.Random
  $obfuscated = ""
  $number = $number.ToString()
  
  for ($i = 0; $i -lt $number.Length; $i++) {
    $obfuscated += $random.Next(0,9)
  }

  return $obfuscated
}
```

## Payloads

```powershell
function Obfuscate-Payload {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory = $true)]
        [string]$Payload
    )

    # Convert the payload string to a byte array
    $payloadBytes = [System.Text.Encoding]::Unicode.GetBytes($Payload)

    # XOR the payload bytes with a random byte
    $key = Get-Random -Minimum 0 -Maximum 255
    $obfuscatedBytes = $payloadBytes -bxor $key

    # Convert the obfuscated bytes back to a string
    $obfuscatedPayload = [System.Convert]::ToBase64String($obfuscatedBytes)

    # Return the obfuscated payload and the key used for obfuscation
    return @{
        Payload = $obfuscatedPayload
        Key = $key
    }
}
```

AES Encrypt and Decrypt

```powershell
# Define a function for encrypting data using AES
function Encrypt-AES {
    param (
        [parameter(Mandatory=$true)]
        [byte[]]$Data,
        [parameter(Mandatory=$true)]
        [byte[]]$Key,
        [parameter(Mandatory=$true)]
        [byte[]]$IV
    )
    $AES = New-Object System.Security.Cryptography.AesCryptoServiceProvider
    $AES.Key = $Key
    $AES.IV = $IV
    $Encryptor = $AES.CreateEncryptor()
    $EncryptedData = $Encryptor.TransformFinalBlock($Data, 0, $Data.Length)
    $AES.Clear()
    return $EncryptedData
}

# Define a function for decrypting data using AES
function Decrypt-AES {
    param (
        [parameter(Mandatory=$true)]
        [byte[]]$Data,
        [parameter(Mandatory=$true)]
        [byte[]]$Key,
        [parameter(Mandatory=$true)]
        [byte[]]$IV
    )
    $AES = New-Object System.Security.Cryptography.AesCryptoServiceProvider
    $AES.Key = $Key
    $AES.IV = $IV
    $Decryptor = $AES.CreateDecryptor()
    $DecryptedData = $Decryptor.TransformFinalBlock($Data, 0, $Data.Length)
    $AES.Clear()
    return $DecryptedData
}
```
