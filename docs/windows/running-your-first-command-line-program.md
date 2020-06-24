# Running your first command-line program

The command line is very straightforward.  You have to remember that it's a
precursor to the GUI, so it's much more simple.

A shell/terminal like Windows Command Prompt or Powershell is just some place
were you write what you want some programs to do.  You also get a few build-in
"special programs" like `cd` (change directory).

I'll get to "how to install" in a bit, but for now let's assume liquidctl has
already been set up.

If you want to list all devices, just type and hit enter:

    liquidctl list

If you want to list all devices showing a bit more information:

    liquidctl list --verbose

If you want to initialize all devices:

    liquidctl initialize all

If you want to show the status information:

    liquidctl status

To change say the pump speed to 42%:

    liquidctl set pump speed 42

It can get slightly less English-looking in some cases, but still nothing too
complex.

For example, to set the fans to follow the profile defined by the points (25°C
-> 10%), (30°C -> 50%), (40°C -> 100%):

    liquidctl set fan speed 25 10 30 50 40 100

_(The profiles run on the device, and therefore can only refer to the internal
liquid temperature sensor)._

Links to specific documentation for each device family can be found in the
[list of _Supported Devices_].  These will list which specific features and
attributes are supported by each device, and provide some additional examples.

[list of _Supported Devices_]: ../../README.md#supported-devices

## Setting up liquidctl

First, there's no installer (for a number of reasons that aren't important
right now).  But we do have pre-built executables so you don't need to install
Python and libraries first.  This is the easiest way for non-programmers on
Windows to use liquidctl, so it's what I'll cover here (see [notes] for how to
install it the Pythonic way).

[notes]: #notes

Next, you should know about the concept of the `PATH` variable.  This is how
the shell, such as Windows Command Prompt or Powershell, finds executables when
you type `liquidctl`.

The idea is that PATH (this environment variable you can configure) is a list
of directories where there are programs that you want the shells to find; in
Windows directories in PATH are separated by semicolons.

You can read more about the PATH variable on its [Wikipedia entry].
Unfortunately I can't find any up-to-date guide from Microsoft on how to set
your PATH (I guess you're supposed to just guess where the setting for it is
and how it works), so instead take a look at this [guide from Aseem Kishore].

[Wikipedia entry]: https://en.wikipedia.org/wiki/PATH_(variable)
[guide from Aseem Kishore]: https://helpdeskgeek.com/windows-10/add-windows-path-environment-variable/

Anyway, getting back to running programs on a shell.  If an executable
(`liquidctl.exe`) is in a directory that is listed in your PATH variable, then
typing

    liquidctl

in any shell (like Windows Command Program) will just work.  You don't even
need the ".exe" suffix.

It's also possible to run programs not in the directories listed in PATH (this
is commonly referred to as running "programs not in PATH"): you just need to
specify a absolute or relative path to the executable.

So there are three ways of setting up `liquidctl.exe`:

* Place it somewhere sensible (personally I use the base `C:\Program Files\`
  directory) and make sure that it is in the PATH (which it normally isn't).

* Place it somewhere already in the PATH; I don't recommend this because in
  most cases you'd be placing it into an internal Windows directory or in the
  directory of some other program.

* Place it anywhere you like and either navigate to it via the shell or specify
  relative/absolute paths to it; I don't recommend this because it's annoying
  and not how command-lines/shells are supposed to be used.

The last stable version of liquidctl can be found in the [_Releases_ page].

[_Releases_ page]: https://github.com/jonasmalacofilho/liquidctl/releases

New drivers may not yet be a part of a stable release (which is the case with
the Kraken X53 as of 24 June 2020).  If that's the case, you can use one of the
automatic builds.

All code in the repository is automatically tested and built for Windows.  The
executable for the latest code in the main code branch can be found at [current
build].  You can also browse all recent builds for all branches and features
been worked on in the [build history]; the executables are in the "artifacts"
tab.

[current build]: https://ci.appveyor.com/project/jonasmalacofilho/liquidctl/branch/master/artifacts
[build history]: https://ci.appveyor.com/project/jonasmalacofilho/liquidctl/history

## Final words

While you should be able to use liquidctl with just these tips, I still
recommend you take a look at the rest of the [README], the documents in [docs]
and, also, the output of:

    liquidctl --help

[README]: ../../README.md
[docs]: ..

## Notes

### The Pythonic way to install liquidctl

To install liquidctl the Pythonic way, first install Python (3.8 recommended),
with the option to add Python to PATH.  Then install liquidctl using pip:

    pip install liquidctl

Finally, install libusb, which unfortunately has to be done manually.  The
libusb DLLs can be found in [libusb/releases](https://github.com/libusb/libusb/releases)
(part of the `libusb-<version>.7z` files) and the appropriate (e.g. MS64)
`.dll` and `.lib` files should be extracted to the system or python
installation directory (e.g.  `C:\Windows\System32` or `C:\Python38`).

There is a [known issue in PyUSB](https://github.com/pyusb/pyusb/pull/227) that
causes errors when the devices are released; the solution is to either manually
patch PyUSB or stick to libusb 1.0.21.

### About this document

This document was originally a response to a direct message:

> Hi. How are you? Hope you're staying safe and well. I just wanted to know of
> there is a windows gui for liquidctrl?
> I have zero experience with command line stuff and I don't entirely understand
> it... also most of the guides are from late 2018 or early 2019.
> And i just bought a x53 kraken.
