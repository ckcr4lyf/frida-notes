# Setup

## Python virtualenvs

We will use virtual env to install frida to play nice with system python and other crap that pip does

```
python3 -m venv frida-venv
source frida-venv/bin/activate
pip3 install -U setuptools
pip3 install frida-tools objection
```

In the future, you should only need to do `source frida-venv/bin/activate`

## Android Phone

Grab the lastest Android `frida-server` from https://github.com/frida/frida/releases . Typically it should be for arm64. The file should be something like `frida-server-16.5.6-android-arm64.xz`

Extract it via `xz -d frida-server-16.5.6-android-arm64.xz`, then make it executable `chmod +x frida-server-16.5.6-android-arm64`

Then push to phone via `adb push frida-server-16.5.6-android-arm64 /data/local/tmp/`
