


# ESPHome: Initial Setup
` esphome wizard dos.yaml `
* name: dos
* type: ESP32
* board: az-delivery-devkit-v4
* WIFI SSID
* WIFI Password
* OTA Password

## Test
 ` esphome run dos.yaml `
 --> COM[x] auswählen


# Integration in Homeassistant

* Einstellungen --> Geräte & Dienste --> Integration Hinzufügen
* "ESPHome" auswählen
* Host & Port aus der console eintragen:

![image](/images/api_server.png)

--> sollte "dos.local" nicht funktionieren, versucht die IP


# Türsensor

### Hardware:
* Reed-Kontakt mit 3V3 und G27 verbinden

### Software:
``` yaml
#ESPHome
binary_sensor:
  - platform: gpio # --> https://esphome.io/components/binary_sensor/gpio.html
    name: "Door Sensor"
    pin:
      number: 27
      inverted: true
      mode:
        input: true
        pulldown: true # verhindert ein "in der Luft hängen" und damit zufälliges Ein- und Ausschalten des Sensors
```

### Flashen:
 ` esphome run dos.yaml ` 
 
 * nun sollte in HA ein "Door Sensor" erscheinen, der den Zustand quasi in Echtzeit widerspiegelt
 * in HA Sensor --> Einstellungen --> "Anzeigen als" --> Tür


# LED Licht

### Hardware:
* LED "GNT1" einstecken
* GND --> -
* G14 --> B
* G33 --> G
* G32 --> R

### Software:
``` yaml
#ESPHome
output: # PINs konfigurieren --> LEDC https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/ledc.html
  - platform: ledc
    id: output_component_r
    pin: 32
  - platform: ledc
    id: output_component_g
    pin: 33
  - platform: ledc
    id: output_component_b
    pin: 14

light: # Mapping zur Plattform --> https://esphome.io/components/light/rgb.html
  - platform: rgb
    name: "RGB Light"
    red: output_component_r
    green: output_component_g
    blue: output_component_b

```

# Automatisierungen

### Licht einschalten, wenn Tür geöffnet
``` yaml
# HA
# Hint: check & change IDs
alias: DOS.DoorOpened
description: ""
trigger:
  - type: opened
    platform: device
    device_id: 88ca177497d62f95df7ff64ebc6e9a90 
    entity_id: binary_sensor.door_sensor
    domain: binary_sensor
condition: []
action:
  - service: light.turn_on
    data:
      transition: 3
      brightness_pct: 100
      color_temp: 222
    target:
      device_id: 88ca177497d62f95df7ff64ebc6e9a90
mode: single
```

### Licht ausschalten, wenn Tür geschlossen
``` yaml
# HA
# Hint: check & change IDs
alias: DOS.DoorClosed
description: ""
trigger:
  - type: not_opened
    platform: device
    device_id: 88ca177497d62f95df7ff64ebc6e9a90
    entity_id: binary_sensor.door_sensor
    domain: binary_sensor
    for:
      hours: 0
      minutes: 0
      seconds: 5
condition: []
action:
  - service: light.turn_off
    data:
      transition: 3
    target:
      device_id: 88ca177497d62f95df7ff64ebc6e9a90
mode: single

```


### Alarm, wenn ARMED
``` yaml
# HA
alias: DOS.DoorOpened
description: ""
trigger:
  - type: opened
    platform: device
    device_id: 88ca177497d62f95df7ff64ebc6e9a90
    entity_id: binary_sensor.door_sensor
    domain: binary_sensor
condition: []
action:
  - choose:
      - conditions:
          - condition: state
            entity_id: input_boolean.armed
            state: "on"
        sequence:
          - service: persistent_notification.create
            data:
              message: Einbrecher!
              title: ACHTUNG
    default:
      - service: light.turn_on
        data:
          transition: 3
          color_temp: 222
          brightness_pct: 100
        target:
          device_id: 88ca177497d62f95df7ff64ebc6e9a90
mode: single

```

### Achtung: Funktionsweise jetzt abhängig von bestehender Verbindung zw. HA & ESP32 !
### Das geht besser! :) 

## Automatisierung via ESPHome

* Details [hier](https://esphome.io/guides/automations.html).

* ESPHome Code um ID und Automatisierung erweitern:
``` yaml
#ESPHome
#  name: "RGB Light"
   id: rgblight

# ...

#    name: "Door Sensor"
    on_press:
      then:
        - light.turn_on:
            id: rgblight
            brightness: 100%
            red: 100%
            green: 100%
            blue: 100%
            transition_length: 0.5s
    on_release:
      then:
        - delay: 3s
        - light.turn_off:
            id: rgblight
            transition_length: 2s

```





[next --> ](/07_Ausblick.md)