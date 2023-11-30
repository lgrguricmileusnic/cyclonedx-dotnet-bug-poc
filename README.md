# cyclonedx-dotnet-bug-poc

Proof of concept for a cdxgen dotnet tool bug, regarding:
https://github.com/CycloneDX/cyclonedx-dotnet/issues/758


Solution structure
```
┌───────────────────────┐             ┌───────────────────────────┐             ┌───────────────────────┐
│  dependent_solution   | ----------> │  dependent_package nupkg  | ----------> │  dependency nupkg     |                 
└───────────────────────┘             └───────────────────────────┘             └───────────────────────┘
           |
           |              ┌───────────────────────┐
           └------------->| dependency csproj     |
                          └───────────────────────┘
```

![image](https://github.com/lgrguricmileusnic/cyclonedx-dotnet-bug-poc/assets/35634240/4c68479a-1bd1-49f9-9da5-00d67b385bf0)

After running:
```
dotnet-CycloneDX .\dependent_solution.sln --json -r -dgl -o .\bom
```

An error is returned:
```
Found the following local nuget package cache locations:
    C:\Users\some_user\.nuget\packages\

» Solution: C:\Users\some_user\source\cdxgen_bug_poc\dependent_solution\dependent_solution.sln
  Getting projects

» Analyzing: C:\Users\some_user\source\cdxgen_bug_poc\dependent_solution\dependent_solution.csproj
  Getting project references

» Analyzing: C:\Users\some_user\source\cdxgen_bug_poc\dependency\dependency.csproj
  Getting project references
  No project references found

» Analyzing: C:\Users\some_user\source\cdxgen_bug_poc\dependency\dependency.csproj
  Getting project references
  No project references found
  2 project(s) found


» Analyzing: C:\Users\some_user\source\cdxgen_bug_poc\dependent_solution\dependent_solution.csproj
  Attempting to restore packages
  Packages restored
Dependency (dependency) with version range ([1.0.0, )) referenced by (Name:dependent_package Version:1.0.0) did not resolve to a specific version.


» Analyzing: C:\Users\some_user\source\cdxgen_bug_poc\dependency\dependency.csproj
  Attempting to restore packages
  Packages restored
  No packages found
Unable to locate valid bom ref for dependency [1.0.0, ) referenced by (Name: dependent_package Version: 1.0.0)
```
