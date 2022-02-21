# OpenTK Website
This is the repository for the OpenTK Website at https://opentk.net

# Building
The builder assumes that you have a directory structure like so
```
<root folder>
      |
     / \
    /   \
   /     \
website  Website_GhPages
```

In this diagram, website is the clone directory for the master branch. Use the Batch Files to build. The finished site is exported to Website_GhPages

# Batch Files
- **_BuildSite.cmd**: runs `docfx build`, which builds the site (excluding metadata) and exports static HTML to the `../Website_GhPages` folder
- **_RebuildMetadata.cmd**: runs `docfx metadata`, which builds all of the projects in the submodule directory ([`opentk`](https://github.com/opentk/opentk)) and creates API Reference from them
- **_UpdateMetadata.cmd**: pulls all changes in the submodule ([`opentk`](https://github.com/opentk/opentk)), then runs `_RebuildMetadata.cmd`
- **_ServeSite.cmd**: runs `docfx serve`, which fully rebuilds the site (including metadata) and then serves it at port 8080.
