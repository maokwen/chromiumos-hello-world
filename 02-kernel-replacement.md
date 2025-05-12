## Upgrade to kernel 6.1

Seach keyward `kernel` in `ChromiumOS > Guides` page, found [Kernel Development]( https://www.chromium.org/chromium-os/developer-library/guides/kernel/kernel-development/) Page.

```
~$ cd ~/chromiumos/src/third_party/kernel
~$ cc-env
```

Check the versions in the workspace:

```sh
chromiumos/src$ fd "^kernel-6_1$"
overlays/overlay-arm64-generic/profiles/kernel-6_1/
overlays/overlay-amd64-generic/profiles/kernel-6_1/
overlays/overlay-arm-generic/profiles/kernel-6_1/

chromiumos/src$ fd "^kernelconfig$"
third_party/kernel/v5.15/chromeos/scripts/kernelconfig
third_party/kernel/pkvm-ia-5.19/chromeos/scripts/kernelconfig
third_party/kernel/v4.19/chromeos/scripts/kernelconfig
third_party/kernel/v5.10/chromeos/scripts/kernelconfig
third_party/kernel/upstream/chromeos/scripts/kernelconfig
third_party/kernel/v6.1/chromeos/scripts/kernelconfig
third_party/kernel/v4.4/chromeos/scripts/kernelconfig
third_party/kernel/v4.14/chromeos/scripts/kernelconfig
third_party/kernel/v5.4/chromeos/scripts/kernelconfig
```

Check the current kernel version:

```sh
chromiumos/src$ cat overlay|s/overlay-amd64-generic/profiles/base/make.defaults | grep 'kernel-'
USE="${USE} legacy_keyboard legacy_power_button sse kernel-5_15" # current version 5.15
```

Build the 6.1 kernel:

```sh
# remove build packages
chromiumos/src$ sudo rm chroot/build/amd64-generic/packages/sys-kernel/chromeos-kernel-*.tbz2
# unmerge the old version
chromiumos/src$ cros_sdk -- emerge-$BOARD --unmerge chromeos-kernel-5_15
# merge the new version
chromiumos/src$ cros_sdk -- emerge-$BOARD --install chromeos-kernel-6_1
# rebuild the image
cros build-image --board=${BOARD} --no-enable-rootfs-verification test
```

Change overlay, replace all `5_15`:

```sh
chromiumos/src$ rg -l '5_15' overlays/overlay-amd64-generic \
  | xargs -r sed -i 's/5_15/6_1/g'

chromiumos/src$ find . -name '*5_15*' -print0 \
    | xargs -0 -I{} bash -c 'mv "$1" "${1//5_15/6_1}"' _ {}
```

Rebuild the image.
