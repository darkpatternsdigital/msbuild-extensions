# DarkPatterns.Build.Autoformat

Adds autoformat tooling to MSBuild projects via dotnet format.

1. In a .NET project, add a package reference to
   `DarkPatterns.Build.Autoformat`.

2. Set `PrivateAssets="All"` on the PackageReference to prevent the Autoformat
   library from being referenced in the build outputs.

3. Configure your `.editorconfig` to match rules for your .NET coding standard.
   See [Microsoft's .editorconfig documentation][ms-editorconfig] for more
   details.

For solution-wide settings:

1. Add the reference to `DarkPatterns.Build.Autoformat` via the
   `Directory.Build.props` to ensure autoformat is set for all projects.

2. Ensure `SolutionRoot` is set within the `Directory.Build.props` as follows:

	```xml
	<SolutionRoot>$(MSBuildThisFileDirectory)</SolutionRoot>
	```

3. Place the `.editorconfig` in the same folder as your Directory.Build.props.

## Configuration

### Properties

| Name | Decription | Default |
| :--- | :--------- | :------ |
| EnforceCodeStyleInBuild | [See Microsoft's documentation for EnforceCodeStyleInBuild][ms-enforcecodestyleinbuild] | `true` |
| AnalysisLevel | [See Microsoft's documentation for AnalysisLevel][ms-analysislevel] | `latest-Recommended` |
| LintSkip | Skips the Lint target entirely if set to `true` | `false` |
| LintEnforceNoChanges | When `true`, will not update files but instead will cause an error if not already in the correct format | `true` if Configuration is `Release` |

### Targets

#### Lint

Runs before build. Executes `dotnet format` for the project being built if `LintEnforceNoChanges` is not true, otherwise `dotnet format --verify-no-changes`.

[ms-editorconfig]: https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/configuration-files#editorconfig
[ms-enforcecodestyleinbuild]: https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#enforcecodestyleinbuild
[ms-analysislevel]: https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#analysislevel