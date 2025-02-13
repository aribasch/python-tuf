# Release process

* Ensure you have a backup of all working files and then remove files not tracked by git
  `git clean -xdf`. **NOTE**: this will delete all files in the tuf tree that aren't
  tracked by git
* Ensure `docs/CHANGELOG.md` contains a one-line summary of each [notable
  change](https://keepachangelog.com/) since the prior release
* Update `tuf/__init__.py` to the new version number "A.B.C"
* Test packaging, uploading to Test PyPI and installing from a virtual environment
  (ensure commands invoking `python` below are using Python 3)
  * Remove existing dist build dirs
  * Create source dist and wheel `python3 -m build`
  * Sign source dist `gpg --detach-sign -a dist/tuf-A.B.C.tar.gz`
  * Sign wheel `gpg --detach-sign -a dist/tuf-A.B.C-py3-none-any.whl`
  * Upload to test PyPI `twine upload --repository testpypi dist/*`
  * Verify the uploaded package at https://test.pypi.org/project/tuf/:
    Note that installing packages with pip using test.pypi.org is potentially
    dangerous (as dependencies may be squatted): download the file and install
    the local file instead.
* Create a PR with updated `CHANGELOG.md` and version bumps
* Once the PR is merged, pull the updated `develop` branch locally
* Create a signed tag matching the updated version number on the merge commit
  `git tag --sign vA.B.C -m "vA.B.C"`
  * Push the tag to GitHub `git push origin vA.B.C`
* Create a new release on GitHub, copying the `CHANGELOG.md` entries for the
  release
* Create a package for the formal release
  (ensure commands invoking `python` below are using Python 3)
  * Remove existing dist build dirs
  * Create source dist and wheel `python3 -m build`
  * Sign source dist `gpg --detach-sign -a dist/tuf-A.B.C.tar.gz`
  * Sign wheel `gpg --detach-sign -a dist/tuf-A.B.C-py3-none-any.whl`
  * Upload to PyPI `twine upload dist/*`
  * Verify the package at https://pypi.org/project/tuf/ and by installing with pip
* Attach both signed dists and their detached signatures to the release on GitHub
* `verify_release` should be used to make sure the release artifacts match the
  git sources, preferably by another developer on a different machine.
* Announce the release on [#tuf on CNCF Slack](https://cloud-native.slack.com/archives/C8NMD3QJ3)
* Ensure [POUF 1](https://github.com/theupdateframework/taps/blob/master/POUFs/reference-POUF/pouf1.md), for the reference implementation, is up-to-date
