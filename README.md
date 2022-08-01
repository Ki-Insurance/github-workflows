# github-workflows
Shared reusable github workflows


### How to create a new release/tag

1. Make your changes, commit them normally
2. Check out the commit, tag with `git tag v0.0.1` (or which ever tag you want to release) and then `git push --tags`
3. After you make sure it working as you expect check out the commit of v0.0.1 again, then update the major version that covers that tag with: `git tag -f v0` (this command overwrites the previous v0 so we use a `-f` flag) and then `git push --tags -f` (similarly we force the new flag on the remote)
