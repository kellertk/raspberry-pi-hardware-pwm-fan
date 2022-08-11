# raspberry-pi-hardware-pwm-fan

A simple python script to control a PWM fan attached to your Raspberry Pi with
one of the Pi's hardware PWM timers. These PWM timers are implemented in
hardware so they don't take resources from the CPU, and their timing is far more
predictable.

There are some caveats to using the hardware PWMs on a Pi. There are only two
channels available and both are in use for audio normally. See
[/boot/overlays/README](https://github.com/raspberrypi/firmware/blob/master/boot/overlays/README) for more information. You must enable the `pwm` or the `pwm-2chan`
overlay for this to work.

## Installation

Note: adjust these for your installation.

1. Clone this repository
2.  `pip install -r requirements.txt`
3.  `cp hardware_pwm_fan /home/pi/scripts && chmod +x /home/pi/scripts/hardware_pwm_fan`
4. Edit `hardware-pwm-fan.service` to set your fan's set points
5. Copy and enable the systemd unit
  ```
  sudo cp hardware-pwm-fan.service /etc/systemd/system
  sudo systemctl daemon-reload
  sudo systemctl enable hardware-pwm-fan.service --now
  ```

## Hardware considerations

By default, GPIO 18 is used for PWM0 and 19 is used for PWM1, but these pins
share functions with PCM. GPIO 12 and 13 are better choices for PWM. Use
`dtoverlay=pwm,pin=12,func=4` or `dtoverlay=pwm,pin=13,func=4` if you want to
use those pins; otherwise you can just use `dtoverlay=pwm`. The `-2chan` variant
is if you want to configure both PWM channels for some reason.
