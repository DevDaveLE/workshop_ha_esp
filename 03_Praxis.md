

## Armed-Switch:
* Einstellungen --> Helfer --> Helfer erstellen --> Umschalten erstellen --> "armed" / "mdi:shield-home"

## Armed-Automatisierung:
* Einstellungen --> Automatisierungen & Szenen --> Neu 

``` yaml

alias: ARMED.StateChanged
description: ""
trigger:
  - platform: state
    entity_id:
      - input_boolean.armed
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
              message: "ARMED "
      - conditions:
          - condition: state
            entity_id: input_boolean.armed
            state: "off"
        sequence:
          - service: persistent_notification.create
            data:
              message: DISARMED
mode: single
```

## Automatisierung testen
* Switch umschalen --> Benachrichtigung erscheint

## Automatisierung debuggen
* ARMED.StateChanged --> Traces anschauen

[next --> ](/04_ESP32.md)