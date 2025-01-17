# minimega Command and Control (cc) Component

```
type: cc
exe:  phenix-scorch-component-cc
```

## Metadata Options

```
metadata:
  vms:
  - hostname:
    configure:
    - type: exec # can be exec, background, send, or recv
      args: whoami # a simple string of args (not an array)
      wait: <bool> # wait for cmd to be executed by VM (default: false)
      validator: <bash script to validate exec results>
    start: [] # same array of keys as above
    stop: [] # same array of keys as above
    cleanup: [] # same array of keys as above
```

> The validator is only used when `type = exec` and forces `wait = true`. The
> validator script should be written to process STDIN. Anything the validator
> script writes to STDERR will be available to the user if the validation fails.

## Notes on the `send` Type

When sending a file with `send`, the source and destination can be specified by
separating them with a colon (`:`). If a colon is not present, then it is
assumed that the file should be placed in the same location in the VM as it is
on the phenix host.

For example, if `/tmp/file.txt:/file.txt` is provided, then the file located at
`/tmp/file.txt` on the phenix host will be placed at `/file.txt` in the VM. If
the VM is a Windows VM, this will translate to `C:\file.txt`.

If only `/tmp/file.txt` is provided, then it will be placed at `/tmp/file.txt`
in the VM. If the VM is a Windows VM, this will translate to `C:\tmp\file.txt`.

If either path is a relative path, then it's assumed to be relative to the
`/phenix` directory on the phenix host and/or the VM.

## Notes on the `recv` Type

When receiving a file with `recv`, the source and destination can be specified
by separating them with a colon (`:`). If a colon is not present, then it is
assumed that the file should be placed in the appropriate location on the phenix
host for the current scorch run, loop, and count.

> In most cases, it's recommended to only provide the source path and let the
> file be placed in the appropriate location on the phenix host.

## Example Configuration

```
components:
  - name: disable-eth0
    type: cc
    metadata:
      vms:
      - hostname: foobar
        start:
        - type: exec
          args: ip link set eth0 down
          wait: true
        stop:
        - type: exec
          args: ip link set eth0 up
```
