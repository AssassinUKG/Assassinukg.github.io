---
title: Powershell Obsfucation tricks 
date: 2022-12-05 09:49:00 +00:00
categories: [tips, tricks]
tags: [tips]     # TAG names should always be lowercase
---


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