# About

This tool is helpful to create rootfs for debian-based distributions with reproducable outputs.

# Installation

```
dotnet tool install --global AptTool
```

This tool needs to be run as root (generate-rootfs), so it is recommended to install it into the system path.

```
sudo dotnet tool install AptTool --tool-path /usr/local/bin/
```

# General

The idea is similar to what you see with the Node package manager (npm). An "image.json" file is used to generate a "image-lock.json" file, which is then use to build a rootfs.

Here is an example "image.json" file.

```json
{
    "repositories": [
        {
            "uri": "http://deb.debian.org/debian",
            "distribution": "buster",
            "components": [
                "main",
                "contrib",
                "non-free"
            ]
        }
    ],
    "packages": {
        "network-manager": "latest"
    },
    "preseeds": [
        "pre-seeds/locales.conf",
        "pre-seeds/timezone.conf"
    ]
}
```

Running ```apt-tool install``` will reach out to the defined repositories and calculate all the packages/versions needed to be installed, and generate a "image-lock.json" file.

After you have generated a "image-lock.json" file, you can then generate the rootfs using ```apt-tool generate-rootfs```.

This will not run anything (only extracting debs), so it can be used for foreign architectures. A "/stage2/stage2.sh" file is placed in the rootfs and must be run on the target architecture before the rootfs can be considered complete.

If you are building a rootfs for the same target architecture that you are building on, you may pass the ```--run-stage2``` command to automatically chroot, run the script, and delete "/stage2" automatically.

# License

MIT