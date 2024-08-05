# DarkPatterns.Build.Structure

Ensures build project structure in the DarkPatterns project style, which
leverages solution-wide settings.

1. Create a `Directory.Build.props` in the root of the repository, along with
   the solution, with the following structure:

	```xml
	<?xml version="1.0" encoding="utf-8" ?>
	<Project>
		<PropertyGroup>
			<UseArtifactsOutput>true</UseArtifactsOutput>
			<RootNamespace>Your.Namespace</RootNamespace>
		</PropertyGroup>

		<ItemGroup>
			<PackageReference Include="DarkPatterns.Build.Structure" Version="0.1.0" PrivateAssets="All" />
		</ItemGroup>
	</Project>
	```

	Optionally, also include the following properties:

	```xml
	<PropertyGroup>
		<SolutionRoot>$(MSBuildThisFileDirectory)</SolutionRoot>
		<RepositoryEngineeringDir>$(SolutionRoot)eng/</RepositoryEngineeringDir>
	</PropertyGroup>
	```

## Configuration

### Properties

| Name | Decription | Default |
| :--- | :--------- | :------ |
| UseArtifactsOutput | [See Microsoft's documentation for Artifacts output layout][ms-artifactsoutput]; must be set to `true` | N/A |
| RootNamespace | [See Microsoft's documentation for Common MSBuild project properties][ms-commonprops] | `$(MSBuildProjectName.Replace(" ", "_"))` |
| SolutionRoot[^1] | Should be the directory where the solution exists. Must end with trailing `/`. | Directory containing the Directory.Build.props |
| RepositoryEngineeringDir[^1] | Must be set to `$(SolutionRoot)eng/` | `$(SolutionRoot)eng/` |

[^1]: These properties cannot be used within your project unless explicitly set.

[ms-artifactsoutput]: https://learn.microsoft.com/en-us/dotnet/core/sdk/artifacts-output
[ms-commonprops]: https://learn.microsoft.com/en-us/visualstudio/msbuild/common-msbuild-project-properties