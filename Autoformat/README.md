# DarkPatterns.Build.Autoformat

Adds autoformat tooling to MSBuild projects via `dotnet format`.

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

2. Place the `.editorconfig` in the same folder as your Directory.Build.props.
   Alternatively, be sure to include your `.editorconfig` in the
   `EditorConfigFiles` item.

## Additional recommendations

* Ensure you have a `.gitattributes` file that sets the default line ending for
  a git checkout to prevent end-of-line changes on first build after switching
  branches. For example:

    ```gitattributes
    * text=auto eol=lf
    ```

## Configuration

### Properties

| Name | Decription | Default |
| :--- | :--------- | :------ |
| EnforceCodeStyleInBuild | [See Microsoft's documentation for EnforceCodeStyleInBuild][ms-enforcecodestyleinbuild] | `true` |
| AnalysisLevel | [See Microsoft's documentation for AnalysisLevel][ms-analysislevel] | `latest-Recommended` |
| LintSkipDotnet | Skips the Lint target entirely if set to `true` | `false` |
| LintEnforceNoChanges | When `true`, will not update files but instead will cause an error if not already in the correct format | `true` if Configuration is `Release` |
| DotnetFormatArgs | Additional arguments for the `dotnet format` command. | Empty |

### Targets

#### Lint

Runs before build. Executes `dotnet format` for the project being built if `LintEnforceNoChanges` is not true, otherwise `dotnet format --verify-no-changes`.

#### PrepareLint

Runs before Lint. May be used prior to linting any project to run prerequisite build steps on a solution-wide basis.

[ms-editorconfig]: https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/configuration-files#editorconfig
[ms-enforcecodestyleinbuild]: https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#enforcecodestyleinbuild
[ms-analysislevel]: https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#analysislevel