# homebrew-tap

Personal Homebrew tap. Formulae here are **generated from each project's GitHub
releases** — versions and checksums are never edited by hand.

```sh
brew tap dfontana/tap
brew install dfontana/tap/starjj
```

## Bumping versions

Cut a release in the source repo as usual (e.g. for `starjj`, push a `vX.Y.Z`
tag). That's it — the [`update-formula`](.github/workflows/update-formula.yml)
workflow downloads the new release assets, recomputes the checksums, and commits
the updated formula.

To make releases trigger the tap instantly, the source repo must notify it. In
`starjj`'s release workflow, after the release is published:

```yaml
      - name: Bump Homebrew tap
        run: |
          gh api repos/dfontana/homebrew-tap/dispatches \
            -f event_type=starjj-release \
            -F client_payload[tag]="${{ github.ref_name }}"
        env:
          GH_TOKEN: ${{ secrets.TAP_DISPATCH_TOKEN }}
```

`TAP_DISPATCH_TOKEN` is a fine-grained PAT with **Contents: write** on this repo,
added as a secret in `starjj` (the default `GITHUB_TOKEN` cannot trigger
cross-repo dispatch).

Without that step it still works: a daily backstop job picks up new releases, and
you can always run the workflow manually from the Actions tab. (Note: GitHub
pauses scheduled workflows after 60 days of repo inactivity, so the dispatch is
the reliable path.)

## Adding another project

Copy `scripts/gen-formula.sh`, swap the repo/asset names, and point a workflow at
it the same way.
