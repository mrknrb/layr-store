# A layr store

A store of [layr](https://github.com/mrknrb/layr_flutter) plugins: `index.json` lists every plugin and version, and
each version's package zip lives in its plugin's own GitHub Releases.

## Using this store in layr

```
layr store use <owner>/<this repository>
```

layr then installs from `https://raw.githubusercontent.com/<owner>/<this repository>/main/index.json`.

## Adding a plugin

1. Publish the plugin from layr: My plugins > Share > Publish (or `layr publish <folder> --public`). That makes a
   public repository, a tag `v<version>` and a release with `<id>-<version>.zip`.
2. Ask to be listed: My plugins > Share > Add to a store (or `layr store add <plugin repository> --store
   <owner>/<this repository>`). layr forks this repository, adds the entry to `index.json` and opens a pull request.
3. The check (GitHub Actions) downloads the zip, compares its sha256, and validates its `layr.json`. A maintainer
   merges the pull request.

An entry by hand looks like this (fields copied from the plugin's `layr.json`):

```json
"counter": {
  "name": "Counter", "description": "A button that counts.", "icon": "plus_one", "author": "someone",
  "versions": {
    "0.1.0": {
      "url": "https://github.com/someone/counter/releases/download/v0.1.0/counter-0.1.0.zip",
      "sha256": "…",
      "dependencies": {}, "permissions": ["data"], "needs": [], "layr": "^0.1"
    }
  }
}
```

## Rules

- A listed version never changes and is never removed: publish a new version, or mark a bad one `"yanked": true`.
- Package URLs are absolute (release assets), and the repository must be public so layr can download them.
- Who may merge is GitHub's business: collaborators of this repository.

## The check

`.github/workflows/check.yml` runs `layr store check index.json --base <index.json before the pull request>` on
every pull request. It needs the `layr` command: set the repository variable `LAYR_BIN_URL` (a prebuilt Linux
binary) or `LAYR_REPO` (layr's git URL, built with cargo).
