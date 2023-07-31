# Version Bump Github Action

A simple GitHub Actions to bump the version of Gradle & Maven projects with extended functionality for automatic versioning.

When triggered, this action will look at the commit message of HEAD~1 and determine if it contains one
of `#major`, `#minor`, or `#patch` (in that order of precedence).
If true, it will use Maven/sed to bump your pom's version/Gradle build's by the X.x.x major, x.X.x minor or x.x.X patch
version respectively.
Furthermore, you can define auto commit features such as increase version number on every commit. (e.g.
project-1.0.0-SNAPSHOT-INCREASE -> project-1.0.0-SNAPSHOT150)
(Used gradle fork https://github.com/fzacek/gradle-version-bump-action from @fzacek to implement gradle also into this
multi use version bump action for any gradle and maven project)

For example, a `#minor` update to version `1.3.9` will result in the version changing to `1.4.0`.
The change will then be committed. Plugily Projects is using it on their projects, to see demo, just watch the repos of
them. You can view examples on the examples dir in this repo, too. 

## Sample Usage
See examples dir for more specific examples which also shows you how to implement it into the publishing part to e.g. maven repo. 
```yaml
name: Version Bump
on:
    push:
        branches: [ development ]
    workflow_dispatch:
        inputs:
            auto-version-bump:
                description: 'Should we bump the version?'
                required: true
                default: 'false'
            tags:
                description: 'Tags'
jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            -   name: Checkout Latest Commit
                uses: actions/checkout@v2

            -   name: Bump Version
                id: bump
                uses: Plugily-Projects/version-bump-action@v6
                with:
                    github-token: ${{ secrets.github_token }}
                    git-committer: 'BOT'
                    auto-version-bump: true
            -   name: Print Version
                run: "echo 'New Version: ${{steps.bump.outputs.version}}'"
```

## Supported Arguments

* `github-token`: The only required argument. Can either be the default token, as seen above, or a personal access token
  with write access to the repository
* `git-email`: The email address each commit should be associated with. Defaults to a github provided noreply address
* `git-username`: The GitHub username each commit should be associated with. Defaults to `version-bump[github-action]`
* `git-committer`: Who should be the committer? defined git [BOT] or last committer [USER]
* `auto-version-bump`: Should we bump the version on every commit?
* `auto-version-bump-splitter`: Version splitter for auto bump
* `auto-version-bump-suffix`: Version suffix for auto bump
* `auto-version-bump-higher`: Should after the suffix a number bumped?
* `auto-version-bump-release`: Should on a automatic bump a tag and release created?
* `auto-version-bump-mode`: Mode for auto version, could be major, minor, patch or auto. Default is auto.

## Outputs

* `version` - The after-bump version. Will return the old version if bump was not necessary.

## Licensing

Copyright © [Plugily Projects](https://plugily.xyz)

Copyright © 2021-2022 [Nick Nichols](https://nnichols.github.io/)

Distributed under the [MIT License](https://github.com/nnichols/maven-version-bump-action/blob/master/LICENSE)
