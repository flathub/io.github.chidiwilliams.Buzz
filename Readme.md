# Flatpak build notes

Instructions for building Buzz Flatpak. 

General Flatpak notes https://docs.flathub.org/docs/for-app-authors/submission/

## Build

Use code from https://github.com/chidiwilliams/buzz

Install `req2flatpak` by running `pip install req2flatpak`

1. Prepare `pyptoject.toml` file
- Remove extra source used for Windows
  - Search for `"torch"` remove `tool.poetry.source` and dependency for `"2.2.1+cu121"`
  - Run `poetrty lock`

2. Export `requirements.txt`
- `poetry export --without-hashes --format=requirements.txt > requirements.txt`

3Generate dependency `.json`
- `req2flatpak --requirements-file requirements.txt --target-platforms 312-x86_64 > buzz-pip-dependencies.json`

4. Add / Update `buzz-captions` in the the main manifest. 

5. Build
```commandline
flatpak run org.flatpak.Builder --force-clean --sandbox --user --install --install-deps-from=flathub --ccache --mirror-screenshots-url=https://dl.flathub.org/media/ --repo=repo builddir io.github.chidiwilliams.Buzz.yml
```

If some package is missing during the build, add it manually to the `buzz-pip-dependencies.json` file.
