# OpenDrop: an Open Source AirDrop Implementation

*OpenDrop* is a command-line tool that allows sharing files between devices directly over Wi-Fi. Its unique feature is that it is protocol-compatible with Apple AirDrop which allows to share files with Apple devices running iOS and macOS. 
Currently (and probably also for the foreseeable future), OpenDrop only supports sending to Apple devices that are discoverable by *everybody* as the default *contacts only* mode requires [Apple-signed certificates](https://www.apple.com/certificateauthority/pdf/Apple_AAI_CPS_v6.1.pdf).


## Disclaimer

OpenDrop is experimental software and is the result of reverse engineering efforts by the [Open Wireless Link](https://owlink.org) project.
Therefore, it does not support all features of AirDrop or might be incompatible with future AirDrop versions.
OpenDrop is not affiliated with or endorsed by Apple Inc. Use this code at your own risk.


## Requirements

To achieve compatibility with Apple AirDrop, OpenDrop requires the target platform to support a specific Wi-Fi link layer as well as several libraries.

**Apple Wireless Direct Link.**
As AirDrop exclusively runs over Apple Wireless Direct Link (AWDL), OpenDrop is only supported on macOS or on Linux systems running
an open re-implementation of AWDL.

**Libraries.**
OpenDrop relies on current versions of [OpenSSL](https://www.openssl.org) and [libarchive](https://www.libarchive.org).
macOS ships with rather old versions of the two, so you will need to install newer version, for example, via [Homebrew](https://brew.sh).
In any case, you will need to set the two environmental variables `LIBARCHIVE` and `LIBCRYPTO` accordingly.
For example, use `brew` to install the libraries:
```bash
brew install libarchive openssl@1.1
```
Then set environmental variables:
```bash
export LIBARCHIVE=/usr/local/opt/libarchive/lib/libarchive.dylib
export LIBCRYPTO=/usr/local/opt/openssl@1.1/lib/libcrypto.dylib
```
Linux distributions should ship with more up-to-date versions, so this won't be necessary.


## Installation 

Installation of the python package is straight forward.
After cloning this repository to `<PATH>`, install via `pip3`:
```
pip3 install <PATH>
```


## Usage

We briefly explain how to send and receive files using `opendrop`.
To see all command line options, run `opendrop -h`.

### Sending a File

Sending a file is typically a two-step procedure. You first discover devices in proximity using the `find` command.
Stop the process once you have found the receiver.
```
$ opendrop find
Looking for receivers. Press enter to stop ...
Found  index 0  ID eccb2f2dcfe7  name John’s iPhone
Found  index 1  ID e63138ac6ba8  name Jane’s MacBook Pro
```
You can then `send` a file using 
```
$ opendrop send -r 0 -f /path/to/some/file
Asking receiver to accept ...
Receiver accepted
Uploading file ...
Uploading has been successful
```
Instead of the `index`, you can also use `ID` or `name`.
OpenDrop will try to interpret the input in the order (1) `index`, (2) `ID`, and (3) `name` and fail if no match was found.

### Receiving Files

Receiving is much easier. Simply use the `receive` command. OpenDrop will accept all incoming files automatically and put received files in the current directory.
```
$ opendrop receive
```


## Related Papers

* Milan Stute, Sashank Narain, Alex Mariotto, Alexander Heinrich, David Kreitschmann, Guevara Noubir, and Matthias Hollick. **A Billion Open Interfaces for Eve and Mallory: MitM, DoS, and Tracking Attacks on iOS and macOS Through Apple Wireless Direct Link.** *28th USENIX Security Symposium (USENIX Security ’19)*, August 14–16, 2019, Santa Clara, CA, USA. [Link](https://www.usenix.org/conference/usenixsecurity19/presentation/stute)


## Authors

* **Milan Stute** ([email](mailto:mstute@seemoo.tu-darmstadt.de), [web](https://seemoo.de/mstute))
* **Alexander Heinrich**


## License

OpenDrop is licensed under the **GNU General Public License v3.0**.
We use a modified version of the [`python-zeroconf`](https://pypi.org/project/zeroconf/) package (essentially adding rudimentary IPv6 and AWDL support) which is licensed under the **GNU Lesser General Public License v2.1**.
Both licenses are found in the `COPYING` file.
