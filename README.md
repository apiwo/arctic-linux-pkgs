# arctic-linux-pkgs

Binary package mirror for Arctic Linux. Point an alpm repo config at a
folder under `ALL/` to use it:

```
name = main
url = https://raw.githubusercontent.com/apiwo/arctic-linux-pkgs/main/ALL/main
enabled = yes
priority = 10
sig = optional
```

Every `INDEX` here is signed with [signify](https://man.openbsd.org/signify)
and the signature is the `INDEX.sig` beside it. The public key ships in the
`arctic-base` package, at `/etc/alpm/keys`, so a machine has it before its
first fetch. `INDEX.sha256` says only that the file arrived intact — anyone
who can replace the index can replace the checksum next to it — which is
what the signature is for.

Packages of 100 MiB or more cannot live in a git repository at all. Those are
release assets under the tag `pkgs-x86_64`, indexed by `ALL/big`, whose
`.repo` file points `pkgurl` at the release. Today that is `linux-firmware`
and the LLVM toolchain.

A published name-version-release is never rebuilt in place: a client holding
the older index would fetch the new file, check it against the old checksum
and refuse it. New bytes get a new release number.
