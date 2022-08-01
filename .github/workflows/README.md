# Reusable workflows

Workflows that are reusable match pattern `r-*.yaml`.Following this pattern allows for easy identification on what workflows are reusable and those which are not.

## r-k8s-pr.yaml
This workflow is only compatible with pull-requests, any other workflow trigger type has not been tested.

Steps:
1. Assumes all directories under `inputs.overlays_root` are overlays and for each of these
    1. Run a kustomize build
    1. (Optionally `inputs.enable_flux_diff`) Run a flux diff and comments the output back to the PR. This expects a configuration file named `inputs.gke_config_yaml_file` to be present at the root of the overlay

Basic usage examples can be found [here](../../examples/k8s/)

There are other inputs that can be provided to this workflow to control the behaviour of the linter etc. See the workflow file for details.

## r-update-kustomize-images.yaml
This workflow takes `inputs.images` and updates `inputs.overlay` with those values.

Steps:
1. Create and switch to `inputs.branch` if it doesn't exist
2. Update overlay `images.newTag` in `kustomization.yaml` with `inputs.images.newTag`, using `images.name` to match old & new
3. Push changes to `inputs.branch`
4. Maybe create pull request (If `inputs.create_pr` && `inputs.branch` != `inputs.base`)
5. [Optionally] run job `promote-images` if `inputs.copy_docker_image`
  1. Attempts to pull/push images between `inputs.images.newName` and `inputs.overlay` `images.newName` if they're different (Will require src/dst docker username/password secrets to be set)

Some inputs are required if creating a pull request.

NOTE: `images.newName` is only used in the promote-images job. The `newName` will be preserved when updating overlays with the values from `inputs.images`, only `images.newTag` is updated in this workflow!

Basic usage examples can be found [here](../../examples/update-kustomization-images).