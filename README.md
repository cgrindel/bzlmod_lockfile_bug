# Demonstrate Incorrect Bzlmod Lock File State

Bazel Version: 6.2.0

## Steps

1. Clone this repo.
2. Modify the `MODULE.bazel` upgrading `cgrindel_bazel_starlib` to `0.16.0`.
3. Run `bazel build --nobuild --lockfile_mode=update //...`. Doing a git status shows the `MODULE.bazel` and the `MODULE.bazel.lock` were updated. Perfect.
4. Run `git checkout -- .`  to revert the changes. You now have the original `MODULE.bazel` and `MODULE.bazel.lock`.
5. Modify the MODULE.bazel upgrading `rules_bazel_integration_test` to `0.14.0`.
6. Run `bazel build --nobuild --lockfile_mode=update //...`.

## Expected

Running `git diff MODULE.bazel.lock` would only show changes related to the upgrade of `rules_bazel_integration_test`. 

## Actual

Running `git diff MODULE.bazel.lock` shows changes related to the `rules_bazel_integration_test`
upgrade and the reverted upgrade for `cgrindel_bazel_starlib`.

## Other Observations

- Running `bazel clean` or `bazel clean --expunge` before Step 5 does not work around the issue. The
  incorrect lock file state persists.
- After running through the reproduction steps and reverting the changes to `MODULE.bazel` and
  `MODULE.bazel.lock`, running `bazel build --nobuild --lockfile_mode=update //...` does not change
  the `MODULE.bazel.lock`.
