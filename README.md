# bindown

[![godoc](https://godoc.org/github.com/WillAbides/bindown?status.svg)](http://godoc.org/github.com/WillAbides/bindown)
[![Go Report Card](https://goreportcard.com/badge/github.com/WillAbides/bindown)](https://goreportcard.com/report/github.com/WillAbides/bindown)
[![ci](https://github.com/WillAbides/bindown/workflows/ci/badge.svg)](https://github.com/WillAbides/bindown/actions?query=workflow%3Aci+branch%3Amaster+event%3Apush)

**bindown** is a command-line utility to download, verify and install binary files. It is intended to be used in
development and ci environments where it is important to guarantee the same version of the same binary is downloaded
every time.

## Config

`bindown` is configured with a yaml file. By default it uses a file named
`bindown.yml` in the current directory.

### Downloader values

#### os 
_required_

The operating system this binary is built for. Common values are `windows`, `darwin` and `linux`. `macos` and `osx` are
aliases for `darwin`.

#### arch
_required_

The system architecture this binary is build for. Common values are `amd64`, `386` and `arm`.

#### url
_required_

The url to download from. The url can point to either the binary itself or an archive containing the binary.

#### checksum
_required_

The sha256 hash of the download. Often projects will publish this in a checksums.txt file along with the downloads. You
can get the value from there or run `bindown config update-checksums <bin-name>` to have `bindown` populate this
automatically.

#### archive path

The path to the binary once the downloaded archive has been extracted. If the download is just the unarchived binary,
this should just be the downloaded file name.

_default_: the binary name

#### link

Whether bindown should create a symlink instead of moving the binary to its final destination.

_default_: false

#### bin

What you want the final binary to be called if different from the downloader name.

_default_: the downloader name

### Example

```yaml
downloaders:
  golangci-lint:
  - os: darwin
    arch: amd64
    url: https://github.com/golangci/golangci-lint/releases/download/v1.21.0/golangci-lint-1.21.0-darwin-amd64.tar.gz
    checksum: 2b2713ec5007e67883aa501eebb81f22abfab0cf0909134ba90f60a066db3760
    archive_path: golangci-lint-1.21.0-darwin-amd64/golangci-lint
    link: true
  - os: linux
    arch: amd64
    url: https://github.com/golangci/golangci-lint/releases/download/v1.21.0/golangci-lint-1.21.0-linux-amd64.tar.gz
    checksum: 2c861f8dc56b560474aa27cab0c075991628cc01af3451e27ac82f5d10d5106b
    archive_path: golangci-lint-1.21.0-linux-amd64/golangci-lint
    link: true
  - os: windows
    arch: amd64
    url: https://github.com/golangci/golangci-lint/releases/download/v1.21.0/golangci-lint-1.21.0-windows-amd64.zip
    checksum: 2e40ded7adcf11e59013cb15c24438b15a86526ca241edfcfdf1abd73a5280a8
    archive_path: golangci-lint-1.21.0-windows-amd64/golangci-lint.exe
    link: true
```


## Usage

```
Usage: bindown <command>

Flags:
  --help                                     Show context-sensitive help.
  --configfile="bindown.yml|bindown.json"    file with bindown config
  --cellar-dir=STRING                        directory where downloads will be cached

Commands:
  version

  download <target-file>
    download a bin

  config format
    formats the config file

  config update-checksums <target-file>
    name of the binary to update

  config validate <bin>
    validate that downloads work

Run "bindown <command> --help" for more information on a command.
```
