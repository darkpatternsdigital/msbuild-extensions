# DarkPatterns.Build.Pnpm

Add build tooling to MSBuild to support PNPM projects!

1. Create a new pnpm project via `pnpm init` or other mechanism.
2. Create a new msbuild project in the same folder as the `package.json` and set
   the `Sdk` to match this package and the latest version. For example:

	```xml
	<Project Sdk="DarkPatterns.Build.Pnpm/0.1.0">
	```

3. Set the corresponding settings, including `PnpmRootPath`, `PnpmBuildScript`, etc.

## Configuration

### Properties

| Name | Decription | Default |
| :--- | :--------- | :------ |
| PnpmBuildScript | The command line to run during the build phase | `pnpm run --if-present build` |
| PnpmTestScript | The command line to run for `dotnet test` | `pnpm run --if-present test` |
| PnpmLintScript | The command line to run for lint checks | `pnpm run --if-present lint` |
| PnpmRootPath | The path to the workspace root for the PNPM project. Should end with a `/` | `$(SolutionRoot)` or `$(ProjectDir)` |

### Items

| Name | Decription | Default |
| :--- | :--------- | :------ |
| CompileOutputs | The files that will be created by the build process | None; required to run build script |
| Compile | The inputs for build and lint | `src/**` |
| CompileConfig[^1] | Inputs that could affect build configuration options.
| RestoreConfig[^1] | The inputs for the node restore configuration. | `$(PnpmRootPath)package.json`, `$(PnpmRootPath)pnpm-lock.yaml`, and `package.json` |
| PnpmPackagedFiles | Additional inputs that will cause the packaged files to change | All CompileConfig items |

[^1]: These will be added to the `Watch` item, for use with `dotnet watch`.

### Targets

#### PnpmInstall

Runs before building, linting, or restoring the project. Works with other
instances of the Sdk to ensure that only one `pnpm install` is running at a time
from within msbuild. Uses `--frozen-lockfile` in Release mode.

#### NodeBuild

Runs before `Build`. Uses `CompileOutputs` for outputs, which must be provided. Executes the `PnpmBuildScript`.

#### Lint

Executes the `PnpmLintScript`.

#### PnpmPack

Runs before `Publish`. Depends on `Build`. Produces an npm-compatible `.tgz` in
the `$(PackageOutputPath)` directory.