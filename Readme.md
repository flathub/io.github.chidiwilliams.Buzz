# Flatpak build notes

Instructions for building Buzz Flatpak. 

General Flatpak notes https://docs.flathub.org/docs/for-app-authors/submission/

## Build

Use code from https://github.com/chidiwilliams/buzz

Install `req2flatpak` by running `pip install req2flatpak`

1. Get all the dependencies
- `uv sync`

2. Export `requirements.txt`
- `uv pip freeze > requirements.txt`

3. Clean requirements file
- Remove `+cu129` from `torch` and `torchaudio`
- Remove `-e file:///home/raivis/Code/buzz`

4. Generate dependency `.json`
- `req2flatpak --requirements-file requirements.txt --target-platforms 313-x86_64 > buzz-pip-dependencies.json`

5. Update `io.github.chidiwilliams.Buzz.metainfo.xml` url in the main manifest. 

6. Update `buzz-captions` wheel in the main manifest. 

7. Adjust build command in `buzz-pip-dependencies.json` to get `torch` and `torchaudio` with `+cu129`.

8. Keep the build dependencies as first installation command.

9. Build
```commandline
flatpak run org.flatpak.Builder --force-clean --sandbox --user --install --install-deps-from=flathub --ccache --mirror-screenshots-url=https://dl.flathub.org/media/ --repo=repo builddir io.github.chidiwilliams.Buzz.yml
```