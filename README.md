# apt-test

Prerequires:
`apt-get install build-essential devscripts debhelper dh-make dpkg-dev`

Steps:
1. Create `<package>-<version>`
2. Put sources under `<package>-<version>/usr/local/bin`
3. Create DEBIAN files `<package>-<version>/DEBIAN/...` (can take from `dh_make` or manually)
4. In `<package>-<version>` directory: `dpkg-deb --build <package>-<version>`
5. _(Test install)_: `sudo dpkg -i <package>_<version>_<arch>.deb`
6. In the root dir, create apt dirs: `mkdir -p dists/stable/main/binary-amd64 pool/main`
7. `dpkg-scanpackages pool/main /dev/null | gzip -9c > dists/stable/main/binary-amd64/Packages.gz`
8. Create `Release` file manually in `./dists/stable/`

Local testing:
1. In the root dir in the separate tab: `python3 -m http.server 8000`
2. `echo "deb [trusted=yes] http://localhost:8000/ stable main" | sudo tee /etc/apt/sources.list.d/local-apt-repo.list`
3.  `sudo apt-get update && sudo apt install <package>`

Deploying:
1.  Change in GitHub Pages settings to Deploy from a branch: "master"
2.  `echo "deb [trusted=yes] https://szachovy.github.io/<repository> stable main" | sudo tee /etc/apt/sources.list.d/local-apt-repo.list`
3.  `sudo apt-get update && sudo apt install <package>`

Usage:
`myhelloworld <arg>` outputs `Hello, World! <arg>`
