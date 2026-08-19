# Weather Tile Agent Instructions

These instructions supplement the repository-level `AGENTS.md` for work in `weather/`.

## Sources and distribution

- Canonical source: `weather/open-weather.html`
- Canonical repository URL: <https://github.com/joshualyon/custom-tile-demos/blob/main/weather/open-weather.html>
- Legacy Gist mirror: <https://gist.github.com/joshualyon/3f83f3605c8d1bd431a4876063b37f38>
- Gist filename: `openWeatherTile.html`
- Community release thread: <https://community.sharptools.io/t/weather-tile-open-weather-current-and-forecast/9237>

The community import links use the canonical repository, but existing tile installations may still update from the original Gist. The Gist is therefore a required release mirror, not an independent source or variant.

## Editing and release rules

- Keep `open-weather.html` as the single canonical implementation.
- Preserve the Gist filename `openWeatherTile.html` and mirror the canonical file exactly.
- Use an isolated clone of the Gist. Do not add it as a remote to the canonical repository or combine their unrelated histories.
- A local edit does not authorize an external push.
- When the user explicitly asks to publish or release weather-tile changes, publish the canonical repository first and then synchronize the Gist unless the user explicitly narrows the requested scope.
- Do not report a weather-tile release as complete until both GitHub destinations have been verified.
- Keep `README.md` focused on end users. Internal publishing workflows belong in this file.

## Before publishing

1. Validate `weather/open-weather.html` locally and in the SharpTools Custom Tile preview.
2. Review the exact repository diff and preserve unrelated working-tree changes.
3. Commit and push the canonical repository change first.
4. Confirm the canonical commit is the intended release source.
5. Synchronize and verify the Gist mirror.
6. Update the community release notes when included in the user's requested release scope.

Publishing is an external write. Obtain or already have explicit user authorization before pushing either destination or editing the community thread.

## Gist synchronization runbook

GitHub Gists are Git repositories. Run these commands from anywhere inside the canonical repository:

```sh
weather_repo_root="$(git rev-parse --show-toplevel)"
weather_gist_dir="$(mktemp -d /tmp/open-weather-gist.XXXXXX)"

git clone https://gist.github.com/3f83f3605c8d1bd431a4876063b37f38.git "$weather_gist_dir"
git -C "$weather_gist_dir" status --short
git -C "$weather_gist_dir" log -1 --oneline
```

If authentication is uncertain, verify push access without publishing:

```sh
git -C "$weather_gist_dir" push --dry-run origin main
```

Copy and review the canonical source:

```sh
cp "$weather_repo_root/weather/open-weather.html" "$weather_gist_dir/openWeatherTile.html"

git -C "$weather_gist_dir" diff -- openWeatherTile.html
git -C "$weather_gist_dir" diff --check
git -C "$weather_gist_dir" add openWeatherTile.html
git -C "$weather_gist_dir" diff --cached --check
git -C "$weather_gist_dir" diff --cached --stat
```

If the copied file produces no diff, the Gist is already synchronized and no mirror commit is needed. Otherwise, commit and push it:

```sh
git -C "$weather_gist_dir" commit -m "chore(weather): sync canonical tile"
git -C "$weather_gist_dir" push origin main
```

An HTTPS Git credential or SSH credential with access to the `joshualyon` Gist is required. GitHub CLI authentication is not required when normal Git credentials already work.

## Gist verification

After pushing, fetch the published branch and compare its file directly with the canonical source:

```sh
git -C "$weather_gist_dir" fetch origin main
git -C "$weather_gist_dir" show origin/main:openWeatherTile.html | \
  cmp - "$weather_repo_root/weather/open-weather.html"
```

`cmp` exits successfully without output when the files are identical. Also verify that the community import link still targets the canonical repository.

## Automation options

The isolated clone workflow is preferred for now. It is deterministic, exposes the exact Gist diff, and can be executed automatically by an agent after publication is authorized.

A GitHub Actions mirror could be added later if releases become frequent: trigger on changes to `weather/open-weather.html` on `main`, then push the file to the Gist using a repository secret with Gist write access. This removes a manual release step but introduces a long-lived credential and requires monitoring failed workflow runs.
