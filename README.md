# OpenTK Website
This is the repository for the OpenTK Website at opentk.github.io

TODO: maybe add some more stuff here like a contribution guide?

# Batch Files
- **_BuildSite.cmd**: runs `docfx build`, which builds the site (excluding metadata) and exports static HTML to the `docs` folder
- **_RebuildMetadata.cmd**: runs `docfx metadata`, which builds all of the projects in the submodule directory ([`opentk`](https://github.com/opentk/opentk)) and creates API Reference from them
- **_UpdateMetadata.cmd**: pulls all changes in the submodule ([`opentk`](https://github.com/opentk/opentk)), then runs `_RebuildMetadata.cmd`
- **_ServeSite.cmd**: runs `docfx serve`, which fully rebuilds the site (including metadata) and then serves it at port 8080.