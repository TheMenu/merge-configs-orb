# Merge config orb

This orb is based on the path-filtering-orb [orb](https://github.com/CircleCI-Public/path-filtering-orb). The orb  allow to merge other configuration files present in the project and also trigger the right workflow if there have been changes between the base revision and the current branch.

## Usage

In a `.circleci/config.yaml` located at the root of the project:

Import the orb
```
orbs:
  multiple-configs: swile/merge-configs@0.1.0
```

Use it in a job
```
workflows:
  trigger-workflow:
    jobs:
      - multiple-configs/merge:
          name: Filter by Folder Modifications
          base-revision: master
          mapping: |
            node/.* node/.circleci/config.yaml
            ruby/.* ruby/.circleci/config.yaml
```

- `base-revision`: the branch which will be compared with the current branch
- `mapping`: the path of each folders with its own `config.yaml`, you can also merge more than 1 file in each line (e.g: `.* node/.circleci/config.yaml ruby/.circleci/config.yaml`)

This configuration will compare the diff between master and trigger workflows related to the current diff.

### How to Publish
* Create and push a branch with your new features.
* When ready to publish a new production version, create a Pull Request from _feature branch_ to `main`.
* The title of the pull request must contain a special semver tag: `[semver:<segement>]` where `<segment>` is replaced by one of the following values.

| Increment | Description|
| ----------| -----------|
| major     | Issue a 1.0.0 incremented release|
| minor     | Issue a x.1.0 incremented release|
| patch     | Issue a x.x.1 incremented release|
| skip      | Do not issue a release|

Example: `[semver:major]`

* Squash and merge. Ensure the semver tag is preserved and entered as a part of the commit message.
* On merge, after manual approval, the orb will automatically be published to the Orb Registry.
