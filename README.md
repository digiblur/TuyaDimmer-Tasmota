# TuyaDimmer-Tasmota
Tuya Dimmer pre-compiled binary and modified source for Tasmota

The bin file is designed for a ESP8266 Tuya 1M dimming device utilizing a second MCU doing the dimming of the lights.  The communication is done via serial over the RX/TX lines of the ESP8266 at 9600-8N1.

Video of the Dimmer and Setup - https://youtu.be/fyxxk2NrKG8

### Parts
- [WiFi Dimmer Module](https://amzn.to/2Oizkb7)
- If they don't ship to your country - Canadian and maybe others countries shipping model - [WiFi Dimmer Module](https://amzn.to/2PrkPX5)
- [USB FTDI Adapter to Flash the Unit](https://amzn.to/2Rpu9rx)


### Example HomeAssistant YAML Configuration:
Change the brightness template entities to match your entity name as well as the topic names.

```yaml
- platform: mqtt
  name: "TuyaDimTest"
  state_topic: "stat/TuyaDimTest/POWER"
  command_topic: "cmnd/TuyaDimTest/POWER"
  availability_topic: "tele/TuyaDimTest/LWT"
  brightness_state_topic: "stat/TuyaDimTest/RESULT"
  brightness_command_topic: "cmnd/TuyaDimTest/Dimmer"
  brightness_scale: 100
  brightness_value_template: >
    {% if value_json.Dimmer is defined %}
      {{ value_json.Dimmer }}
    {% else %}
      {% if state_attr('light.tuyadimtest','brightness') == none %}
        0
      {% else %}
        {{ state_attr('light.tuyadimtest','brightness') / 255 * 100 }}
      {% endif %}
    {% endif %}
  qos: 1
  payload_on: "ON"
  payload_off: "OFF"
  payload_available: "Online"
  payload_not_available: "Offline"
  retain: false
```

### Disclaimer
:warning: **DANGER OF ELECTROCUTION** :warning:

The dimmer uses Mains AC power!  There is a danger of electrocution if not installed properly. If you don't know how to install it, please call an electrician. Remember: _**SAFETY FIRST**_. It is not worth to risk yourself, your family and your home if you don't know exactly what you are doing. Never try to flash, open, or service the dimmer device while it is connected to any power source.

We don't take any responsibility nor liability for using this software nor for the installation or any tips, advice, videos, etc. given by anyone on this site or any other site.
