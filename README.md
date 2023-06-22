# porla-pontos
This is an extension to [`porla`](https://github.com/MO-RISE/porla)

## What

This extension provides the necessary tools to be compatible with the dtaa format used by [pontos-hub](https://github.com/MO-RISE/pontos-hub)

### Built-in functionality

* **canboat2pontos**

  Converts output from canboat's `analyzer --json` to pontos format, see [porla-nmea](https://github.com/MO-RISE/porla-nmea). Expects a single argument, the `vessel_id`. Expects input in the form `<epoch> <canboat json output>`. Outputs `<topic> <json payload>`, suitable for input to `mqtt-cli`, see [porla-mqtt](https://github.com/MO-RISE/porla-mqtt).

### 3rd-party tools

N/A

## Usage

### Examples
```yaml
version: '3.8'

services:
services:
    source_1:
        image: ghcr.io/mo-rise/porla
        network_mode: host
        restart: always
        command: ["socat UDP4-RECV:1457,reuseaddr STDOUT | to_bus 1"]

    transform_1:
        image: ghcr.io/mo-rise/porla-nmea
        network_mode: host
        restart: always
        command: ["from_bus 1 | analyzer --json | timestamp --epoch | to_bus 2"]

    transform_2:
        image: ghcr.io/mo-rise/porla-pontos
        network_mode: host
        restart: always
        command: ["from_bus 2 | canboat2pontos test_vessel | to_bus 2"]

```
