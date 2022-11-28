# PackAsAnalyzer
MSBuild infrastructure support to include source generators (analyzers) in nuget packages.

## Usage
1. Add the Sdk element that uses the `ViHo.PackAsAnalyzer` msbuild sdk package in your repo root's Directory.Build.props file so that all projects have access to the functionality:
```xml
<Project>
  <!-- Reference the msbuild sdk package from nuget.org which makes it automatically available to all projects in the repo. -->
  <Sdk Name="ViHo.PackAsAnalyzer" Version="1.0.1" />
</Project>
```

2. Add a `ProjectReference` item that points from the packable library to the source generator project and add the `PackAsAnalyzer` metadata to it:
```xml
<ItemGroup>
  <ProjectReference Include="..\analyzer\analyzer.csproj"
                    ReferenceOutputAssembly="false"
                    PackAsAnalyzer="true" />
</ItemGroup>
```

:information_source: Note: If you also want to consume the source generator in your project (instead of just packaging it), add the `OutputItemType=Analyzer` metadata to the `ProjectReference` item.

### Configuration
The following properties can be set in the analyzer project:
- `AnalyzerLanguage`: For C# projects, set to `cs`, for Visual Basic projects, set to `vb`. Omit the property if the analyzer should apply to all assembly families.
- `AnalyzerRoslynVersion`: If the analyzer requires a minimum version of the Roslyn version, set this metadata. As of time of writing, `roslyn4.4` packages are available. If the analyzer should target the very latest version only, set the property to `4.4`.

Example:
```xml
<!-- A source generator that runs if the input is a C# assemblies and when the compiler supports the roslyn4.4 API. -->
<PropertyGroup>
  <AnalyzerLanguage>cs</AnalyzerLanguage>
  <AnalyzerRoslynVersion>4.4</AnalyzerRoslynVersion>
</PropertyGroup>
```