# Microsoft.VisualStudio editor packages, MIT build

`Microsoft.VisualStudio.CoreUtility`, `.Text.Data`, `.Text.Logic` and `.Text.UI` built from the MIT-licensed
sources of this repository (a fork of `microsoft/vs-editor-api`) for net10.0, published to the lextudio GitHub
Packages feed under Microsoft's own package ids (`Microsoft.VisualStudio.CoreUtility`, `.Text.Data`, `.Text.Logic`,
`.Text.UI`) so that packages built for Visual Studio keep resolving their dependencies. Consume them only through a
source mapping that sends these ids to the lextudio feed, never from nuget.org. The version is `17.3.198.1`: the sources are the
August 2022 snapshot, which matches Microsoft's 17.3.198 (2022-08-09) but is a subset of it (e.g. no
`ITextBufferFactoryService2`, TextMate constants or leak-tracker interfaces; measured 2026-11-09: 6 types/members
missing from Text.Data, 41 from CoreUtility, 76 from Text.Logic, up to 197 from Text.UI, many of the latter only moved
between assemblies). A package that needs 17.4 or later is deliberately not satisfied. The assembly version stays
16.0.0.0.

Why not Microsoft's own packages: those are licensed "MICROSOFT SOFTWARE LICENSE TERMS - MICROSOFT VISUAL STUDIO
ADD-ONs and EXTENSIONS" (Visual Studio use only, no redistribution inside another application), while this
repository is MIT.

Differences from upstream sources: only `TextDocumentFileActionEventArgs.cs` of `Text.Data` is replaced, by
`lextudio/Text.Data/TextDocumentFileActionEventArgs.cs`, which adds the newer rename constructor and `OldFilePath` (an addition; nothing is removed).
`Text.UI` builds through `LibreWPF.Sdk` (see `global.json`).

Build: `dotnet pack lextudio/Text.UI/LeXtudio.VSEditor.Text.UI.csproj -c Release -o nupkg` (restores the other
three through project references; the LibreWPF packages come from the `openavalon-github` feed, see `NuGet.config`).
Publish: `.github/workflows/lextudio-packages.yml` (push of a `v*` tag, or manual dispatch).
