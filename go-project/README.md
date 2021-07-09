# go-project
## Installing the CLI
Some ways of installing the CLI:
* `wget` the desired binary from GitHub Releases

```bash
$ wget -c https://github.com/urianchang/sandbox-1/releases/download/cli-0.0.1/go-project_0.0.1_Darwin-x86_64.tar.gz -P /tmp/go-project && \
tar -xzf /tmp/go-project/go-project_0.0.1_Darwin-x86_64.tar.gz -C /tmp/go-project && \
cp /tmp/go-project/hello-world /usr/local/bin && \
rm -rf /tmp/go-project
```

* `npm install` from the GitHub npm registry.

```bash
# You'll need to include this in your ~/.npmrc
$ cat ~/.npmrc
@urianchang:registry=https://npm.pkg.github.com/

$ npm install -g @urianchang/go-project

> @urianchang/go-project@0.0.1 preuninstall /usr/local/lib/node_modules/@urianchang/go-project
> node postinstall.js uninstall

Uninstalled cli successfully

> @urianchang/go-project@0.0.1 postinstall /usr/local/lib/node_modules/@urianchang/go-project
> node postinstall.js install

Copying the relevant binary for your platform darwin
Installed cli successfully
+ @urianchang/go-project@0.0.1
updated 1 package in 2.168s
```

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
2. Update `CliVersion` in cmd/version.go with the new version:

```bash
> git diff cmd/version.go  
...
-var CliVersion = "0.0.0+dev"
+var CliVersion = "0.0.1"
```

3. Update cmd/CHANGELOG.md with the new version and add the version's release date.

```bash
> git diff cmd/CHANGELOG.md 
...
 # CHANGELOG
-## Release: 0.0.0+dev ([master](https://github.com/urianchang/sandbox-1/tree/master/go-project))
+## Release: 0.0.1 (7/8/2021)
```

4. Commit and push your changes to the upstream repository.
5. Create a GitHub Pull Request (PR) and seek approval.
6. When the PR has been approved, create and push a git tag with the format: `cli-<version>`

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

```bash
> git diff cmd/version.go 
...
-var CliVersion = "0.0.1"
+var CliVersion = "0.0.1+dev"
```

10. Update cmd/CHANGELOG.md with a section for the new version.

```bash
> git diff cmd/CHANGELOG.md
...
 # CHANGELOG
+## Release: 0.0.1+dev ([master](https://github.com/urianchang/sandbox-1/tree/master/go-project))
+
```

11. Commit and push your changes to the upstream repository. 
12. Merge the PR into `master`.
