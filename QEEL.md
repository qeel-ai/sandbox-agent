# Qeel's fork of Sandbox Agent

The `qeel` branch is upstream's release tag plus a short queue of commits.
Qeel runs its agents' harnesses in Sandbox Agent; this fork adds the
harnesses upstream does not have yet and publishes the runtime image Qeel uses.

## The commits

- `feat: add <harness> agent` commits are written to be sent upstream as
  pull requests. When upstream ships one, drop ours at the next rebase.
- `qeel:` commits are Qeel-only: image pins and settings, and the
  `qeel-image` workflow.

## Moving to a new upstream release

```sh
git fetch upstream --tags
git rebase --onto vX.Y.Z vOLD qeel   # vOLD: the tag the queue sits on now
cd sandbox-agent/server && cargo test -p sandbox-agent-agent-management \
  && cargo test -p sandbox-agent --test v1_agent_process_matrix
git push --force-with-lease origin qeel
```

Keep the `sandbox-agent` npm package in Qeel on the same upstream version.

## Publishing an image

Tag the `qeel` branch `vX.Y.Z-qeel.N` (N counts up from 1 for each upstream
version) and push the tag. The `qeel-image` workflow builds linux/amd64 and
linux/arm64 and publishes `ghcr.io/qeel-ai/sandbox-agent:X.Y.Z-qeel.N`.
To change only the omp version, bump `OMP_VERSION` in
`sandbox-agent/docker/runtime/Dockerfile.full` and tag the next N.
