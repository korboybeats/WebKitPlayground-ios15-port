# WebKitPlayground iOS 15 Port

Patch repository for porting Lessica's WebKitPlayground changes to:

- iOS `15.0`
- build `19A346`
- WebKit `8612.1.27.0.31`
- upstream source tag `releases/Apple/Safari-15-iOS-15.0`

This repository does not vendor WebKit source. It contains:

- the iOS 15 port patch in [ios15-port.patch](/home/korboy-wsl/WebKitPlayground/ios15-port.patch)
- ABI shim files in [scripts/abi-shims](/home/korboy-wsl/WebKitPlayground/scripts/abi-shims)
- GitHub Actions workflows for patch validation and optional macOS build attempts

## Local Use

Clone the upstream WebKit tag, copy in the shim files, apply the patch, then build:

```sh
git clone --depth 1 --branch releases/Apple/Safari-15-iOS-15.0 https://github.com/WebKit/WebKit.git WebKit-15.0
cp -R scripts WebKit-15.0/
git -C WebKit-15.0 apply ../ios15-port.patch
cd WebKit-15.0
Tools/Scripts/build-webkit --ios-device --release --use-ccache WK_USE_CCACHE=YES ARCHS='arm64 arm64e' GCC_TREAT_WARNINGS_AS_ERRORS=NO OTHER_CFLAGS='$(inherited) -Wno-error -Wno-error=strict-prototypes -Wno-strict-prototypes -Wno-error=deprecated-declarations' OTHER_CPLUSPLUSFLAGS='$(inherited) -Wno-error -Wno-error=deprecated-declarations'
```

## CI

Two workflows are provided:

- patch validation on every push and pull request
- macOS iOS build attempt via manual dispatch

The build workflow is intentionally manual because Apple WebKit device builds are heavy and may exceed GitHub-hosted runner limits.
