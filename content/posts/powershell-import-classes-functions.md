---
title: "Importing Functions, Classes, and Methods From Other PowerShell Files"
date: 2022-07-31T18:20:36-04:00
draft: false
---

I've recently been working on a project that requires me to use PowerShell. I actually feel relatively fluent in PowerShell since it was my main scripting language for a little over a decade while I worked as a sysadmin in highly Windows-centric environments that involved me automating as much of my job as possible in order to be able to do things like sleep on occasion. However, my PowerShell work rarely (read: never) went beyond single file scripts.

With this project being a decent bit more complicated and with the potential for some of the code to be useful in future projects, I wanted to figure out how to actually break things up into useful chunks, just like I would do when writing something in Python. Fortunately, it wasn't terribly difficult to figure out, though, as is typically the case with PowerShell, the documentation was a bit wanting. I had to put information together from a few different resource and go through a little trial-and-error to actually figure it out since Microsoft can never just seem to give clear, concise examples of anything PowerShell-related.

The first thing I needed to realize is that I wanted to create my files not with a normal `.ps1` extension but with a `.psm1` extension to indicate that they were PowerShell modules. Only the file that would normally be executed directly has a `.ps1` extension. I kind of hate this since it makes things more difficult to test individually; in Python, for example, I could just create a main function that executes when:

```python
if __name__ == "__main__":
```

Then I can add things in `main` while building it out that are later ignored when the code is called from elsewhere. PowerShell doesn't offer anything like this, though it's not a huge ordeal. After creating a `.psm1` file, it can contain functions, classes, and/or methods. For example, here's a sample file called `helloFunc.psm1` with just a function:

```powershell
function Write-Hello {
    param(
        [Parameter(Mandatory=$true)][string]$Name
    )

    Write-Output "Hello, $Name."
}
```

And here's a file called `personClass.psm1` with both a class and a couple of methods:

```powershell
class Person {
    [String]$Name
    [int]$Age

    # Constructor.
    Person([String]$Name, [int]$Age) {
        $this.Name = $Name
        $this.Age = $Age
    }

    [String]GreetPerson([String]$PreferredGreeting) {
        if($PreferredGreeting -eq "" -or $null -eq $PreferredGreeting) {
            $PreferredGreeting = "Hello"
        }
        return "$PreferredGreeting, $($this.Name). I can't believe you're $($this.Age) years old."
    }

    [void]HaveBirthday() {
        $this.Age++
    }
}
```

Neither file has an entrypoint, though that's expected since they're designed to be called from somewhere else. Here's the `main.ps1` file which ties them all together:

```powershell
#!/usr/bin/env pwsh
using module ./personClass.psm1
using module ./helloFunc.psm1

Write-Hello -Name "Garrett"

$me = [Person]::new("Garrett", 9000)
Write-Output $me.GreetPerson("Salutations")
$me.HaveBirthday()
Write-Output $me.GreetPerson("Salutations")
```

The most important thing here are the two `using` statements, which specify that I'm going to import the two aforementioned files. Once I do this, I can then call classes, methods, and functions in those files directly.
