# LoopO firmware

This repo contains the source code for the latest LoopO implementation.

## Hardware

The device is based on a pimoroni servo motor board and 4 micrometal motors with encoders. One motor is needed LoopO and the other 3 are used for LoopO-twist, alongside other sensors.

## Software

The code is based on the pimoroni rpi pico boilerplate. I recommend following their instrusctions to fully set up the build environment. The device communicates to the computer through serial.

### Control messages

Numerical codes can be sent to control the device similarly to the robotis dynamixel protocol. Each control message is formed as follows:

#### message syntax

|    ID     |  Command   |     Value     |
|-----------|------------|---------------|
|  1 Byte   |  1 Byte    |    4 Bytes    |
|   char    |    char    |     float     |

#### device ID

| ID | Device   |
|----|----------|
| 1  | extension|
| 2  | twist    |
| 3  | loop     |

#### Command Values

| Command | Ext Action  | Twist action | Loop Action | Required value  |
|---------|-------------|--------------|-------------|-----------------|
|    0    | set enable  | set enable   | set enable  |      0-1        |
|    1    | set control | set control  | set control |    0-1-2-3      |
|    2    | set speed   | set speed    | set speed   |     float       |
|    3    | set position| set position | set position|     float       |
|    4    | set velocity| set velocity | set velocity|     float       |
|    5    |    home     |     home     |     home    |     float       |
|    6    |    none     | set force    | set offset  |     float       |

#### Control Approaches

| Value | Approach |
|-------|----------|
|   0   |  Speed   |
|   1   | position |
|   2   | velocity |
|   3   |  Force (only available to loop)  |

### Status reply

After each message is received and applied a status message is sent back. A feedack message will be provide after any message sent, even malformed or empty ones, the latter is useful for logging purposes. each message is composed of 11 values separated by ":"

|0|1|2|3|4|5|6|7|8|9|10|
|-|-|-|-|-|-|-|-|-|-|-|
extension position|extension status|extension control|twist position|twist offset|twist status|twist control|loop position|loop force|loopp status|loop control|

#### Status table

|value|Meaning|
|-|-|
|0|DISABLED|
|1|IDLE|
|2|HOMING|
|3|MOVING|
|4|LIMIT
|5|STUCK|
|6|GRIPPING (only for loop)|