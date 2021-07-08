# go-project

## Release Process
### Prerequisites
Before starting the release process, please install [`goreleaser`](https://goreleaser.com/intro/),
which we use to create Go binaries and draft a GitHub release.

```bash
# For other installation options, please see: https://goreleaser.com/install/
> brew install goreleaser/tap/goreleaser
> brew install goreleaser
```

Additionally, `goreleaser` requires a GitHub API token saved as the `GITHUB_TOKEN` 
environment variable. You can create a token here: https://github.com/settings/tokens/new. 

```
export GITHUB_TOKEN="GH_API_Token"
```

### Publish GitHub release
1. Create and checkout a branch with the format: `cli/rc<version>`. Example: `cli/rc0.0.1`
2. Update `CliVersion` in cmd/version.go
3. Update cmd/CHANGELOG.md
4. Commit and push your changes to the upstream repository.
5. Create a GitHub Pull Request (PR) and seek approval.
6. When the PR has been approved, create and push a git tab with the format: `cli-<version>`

```bash
> git tag -a cli-0.0.1 -m "CLI release v0.0.1"
> git push origin cli-0.0.1
```

7. Run `goreleaser`, which will create Go binaries in the `dist/` directory, draft a GitHub release,
and upload the binaries to the release draft.

```bash
> cd go-project
> goreleaser release --skip-validate --rm-dist
   • releasing...     
   • loading config file       file=.goreleaser.yml
...
   • release succeeded after 5.88s
```

8. Go to the [GitHub releases page](https://github.com/urianchang/sandbox-1/releases) and update the GitHub release draft with relevant changelog notes. When ready, publish the release. 
9. Update `CliVersion` in cmd/version.go with a `+dev` suffix.
10. Update cmd/CHANGELOG.md
11. Commit and push your changes to the upstream repository. 
12. Merge the PR into `master`.
