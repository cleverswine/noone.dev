# noone.dev

Source for the website, powered by Hugo

Script to get latest version of Hugo:

```bash
#!/usr/bin/env bash

pushd /tmp/

curl -s https://api.github.com/repos/gohugoio/hugo/releases/latest \
| grep "browser_download_url.*hugo_[^extended].*_Linux-64bit\.tar\.gz" \
| cut -d ":" -f 2,3 \
| tr -d \" \
| wget -qi -

tarball="$(find . -name "*Linux-64bit.tar.gz")"
tar -xzf $tarball

chmod +x hugo

mv hugo /usr/local/bin/

popd

location="$(which hugo)"
echo "Hugo binary location: $location"

version="$(hugo version)"
echo "Hugo binary version: $version"
```