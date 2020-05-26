[Russian/Русский](README_RUS.md)

# Rutoken M2M SDK Utils

This project is a bunch of utilities to be used in [Rutoken M2M SDK](https://www.rutoken.ru/products/all/rutoken-m2m/)
to provide a better user experience.

## rt-control

**rt-control** -- control the state of the physical connection of smartcards hosted on
[Rutoken M2M demo board](https://www.rutoken.ru/products/all/rutoken-m2m/)
and the status of smartcard-aware middleware.

### Synopsis

`rt-control <command> [command arguments]`

### Description

**rt-control** utility is used to control the state of the physical connection of smartcards hosted on
[Rutoken M2M demo board -- Rutoken 4990](https://www.rutoken.ru/products/all/rutoken-m2m/)
as well as the status of smartcard-aware middleware. The utility allows to power on and power off smartcards on Rutoken M2M demo board,
start, stop, change log level of pcscd, view current status of controlled entities.

### Commands

##### `-s, --select_devices <device> [<device>...]`

Power on specified devices and enable access to them over the PC/SC interface.
Devices not specified in command will be powered off. The following `device` values are supported:
    * `2010` -- Rutoken 2010
    * `4010` -- Rutoken 4010
    * `21xx` -- MicroSIM reader

Note that enabling both Rutoken 4010 and MicroSIM reader simultaneously is not supported because of Rutoken M2M demo board schematics.

##### `-l, --log_level [-c <ccid_log_level>] [-u <rtuart_log_level>] [-s <rtuartscreader_log_level>]`

Enable pcscd logging. Pcscd gets restarted with `-a -d` options. Additionally with `-c <ccid_log_level>`, `-u <rtuart_log_level>` and `-s <rtuartscreader_log_level>` log level of [CCID](https://ccid.apdu.fr/), [rtuart](https://github.com/AktivCo/rtuart) and [rtuartscreader](https://github.com/AktivCo/rtuartscreader) drivers can be specified. Both drivers use numeric value in range from 0 to 15 to represent log level. See corresponding driver description for more info.

##### `-d, --disable_log`

Disable pcscd logging. Pcscd gets restarted with no options. Logging from IFD handlers is also implicitly disabled.

##### `-p, --pcscd  <start|stop|restart>`

Start, stop or restart pcscd. With pcscd being stopped smartcards can not be accessed over PC/SC. Pcscd gets started with the logging settings previously specified by using `-l` and `-d` command.

##### `-i, --info`

Display information concerning smartcard power status, its availability over PC/SC interface, pcscd running status, and logging level.

##### `-h, --help`

Display help message.

## rt-run-sample

**rt-run-sample** - run executable in the preset environment with pcsc-spy enabled and/or some devices on Rutoken M2M demo board powered off.

### Synopsis

`rt-run-sample [option...] -p <path>`

### Description

**rt-run-sample** utility enables logging of the PC/SC calls using pcsc-spy. If logging is selected, logs are written
to `~/log` directory. It also allows the user to power off some devices on Rutoken M2M demo board so that using executables not expecting multiple smartcards to be connected to PC won't be ambiguous.

### Options

##### `-s`

Enable logging of the PC/SC calls using pcsc-spy. Logs get written to `~/log` directory.

##### `-l`

Redirect executable output to file in `~/log` directory.

##### `-d <device>`

Specify devices that will be powered on and thus accessible over PC/SC during execution.

Option values supported:
    * 2010 -- Rutoken 2010 SoC only is powered on.
    * 4010 -- Rutoken 4010 SoM only is powered on.
    * 21xx -- MicroSIM reader only is powered on.
    * 2010+4010 - both Rutoken 2010 SoC and Rutoken 4010 SoM are powered on.
    * 2010+21xx - both Rutoken 2010 SoC and MicroSIM reader are powered on.

##### `-p <path>`

Specify the executable to run.

##### `-h`

Display help message.

### Notes

The major reason to use this utility in Rutoken M2M SDK is that Rutoken SDK samples distributed with the demo board use the first found PKCS#11 slot, so you can not be sure which smartcard is used in the sample.

## rt-uart-test

**rt-uart-test** - run rtuart_transport_test in preset environment.

### Synopsis

`rt-uart-test [option]`

### Description

**rtuart_transport_test** is one of the artifacts of [rtuart](https://github.com/AktivCo/rtuart) project. This executable performs communication with Rutoken 4010 using the low-level protocol. It won't run properly if rtuart driver is currently used or Rutoken 4010 is powered off. **rt-uart-test** either checks if preconditions to run **rtuart_transport_test** are meet or forces such preconditions.

### Options

##### `-f, --force`

Configure runtime environment to allow running rtuart_transport_test: stops pcscd, powers on Rutoken 4010. Without this option, the utility will check if the environment is OK and, in this case, will run rtuart_transport_test.

##### `-h`

Display help message.
