# Dappy CLI

Public release downloads for the Dappy command-line client and agent daemon.
The application source is maintained separately in a private repository.
Downloading requires no GitHub account. Dappy sign-in and project access are
required to use account and project commands.

**Release status:** the first version, `0.1.0`, is being prepared. No installable
release has been published yet. It will be published after the matching Dappy
backend is deployed. The install commands below apply once that release exists.

## Install

Install Node.js **22.13.0 or newer**, then:

```sh
npm install --global --ignore-scripts https://github.com/haodt/dappy-cli/releases/latest/download/dappy-cli.tgz
dappy --version
dappy --help
```

Use a versioned URL to pin an installation:

```sh
npm install --global --ignore-scripts https://github.com/haodt/dappy-cli/releases/download/v0.1.0/dappy-cli.tgz
```

The package includes its runtime JavaScript dependencies; it has no install
scripts. Use a user-writable Node global installation rather than running npm
with sudo. The same Node package supports native macOS and Linux usage.

## Sign in

From outside a Dappy development checkout, select the production application:

```sh
export DAPPY_API_URL=https://dappy.haodt1990.workers.dev
dappy login
dappy me
dappy projects list
```

Complete the browser sign-in with your Dappy account. A project invitation is
separate from installing the CLI; ask your project administrator for access.
People who only use the web app do not need this CLI or a local runtime.

## Verify or install manually

Each version in [Releases](https://github.com/haodt/dappy-cli/releases) includes:

- `dappy-cli.tgz`: standalone package.
- `dappy-cli.tgz.sha256`: archive checksum.
- `dappy-cli.manifest.json`: version, source commit, build time, Node requirement,
  archive size, and checksums.

Download the archive and checksum from the same versioned release, then:

```sh
shasum -a 256 -c dappy-cli.tgz.sha256
tar -xzf dappy-cli.tgz
node package/dappy.js --version --json
```

Keep the whole extracted package together, including `supervisor.js`.
`--version --json` reports build provenance without sign-in or a network request.
Published versions are never overwritten by our release workflow.

## Host an agent runtime

Agent execution also needs Codex CLI **0.155.0**, installed and authorized
separately. It is not bundled with this download. The new runtime backend must
be deployed before enrollment is available.

After signing in to the intended Dappy account, open **Agents → Runtimes →
Connect runtime** in the application for enrollment instructions. Start with a
foreground daemon, confirm an Online heartbeat, and verify a first agent reply
before configuring background operation. Use a dedicated runtime root and
credential home for production; do not reuse local-test runtime state.

## Update or roll back

Stop the daemon before installing another version, restart it afterward, and
check that its heartbeat is fresh. Credentials and runtime data are stored
outside the package and must be retained. Use a pinned release URL to roll back
to a version compatible with your server. CLI installation does not apply server
migrations or update Codex, and the daemon does not automatically update itself.

Report problems through your Dappy project administrator, including
`dappy --version --json` and the error message. Do not include tokens or private
runtime data.
