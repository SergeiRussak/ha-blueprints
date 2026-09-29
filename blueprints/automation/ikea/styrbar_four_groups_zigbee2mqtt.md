# IKEA STYRBAR — 4 группы через Zigbee2MQTT

Blueprint: `styrbar_four_groups_zigbee2mqtt.yaml`

Он использует стандартные MQTT device triggers STYRBAR из Zigbee2MQTT: `on`, `off`,
`brightness_move_up`, `brightness_move_down`, `arrow_left_click`,
`arrow_right_click`, а также `arrow_left_hold` и `arrow_right_hold`.

## Подготовка helper’ов

Можно подключить готовый package:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

После этого используйте helper’ы:

- `input_text.styrbar_active_group` — начальное значение `none`;
- `input_text.styrbar_last_event` — начальное значение `{}`.

Если packages не используются, создайте два Text helper’а вручную через
Settings → Devices & services → Helpers.

## Поведение

- Короткое ▲/▼/◀/▶ в обычном режиме переключает соответствующую группу.
- Длительное нажатие любой кнопки выбирает её группу и включает режим яркости.
- В режиме яркости короткое ▲ увеличивает яркость включённых ламп активной
  группы, короткое ▼ уменьшает её.
- В режиме яркости короткое ◀ или ▶ только выходит из режима; группа не
  переключается.
- Розетки участвуют в toggle, но пропускаются при изменении яркости.

Короткие события запускаются только для известных action-значений. События
`brightness_stop`, `arrow_left_release` и `arrow_right_release` игнорируются.

## Настройка automation

В blueprint выберите само MQTT-устройство STYRBAR, два helper’а и четыре набора
сущностей. Не выбирайте Battery-сенсор. Для каждой группы можно выбрать любое
сочетание `light.*` и `switch.*`.

Если STYRBAR не появляется среди MQTT-устройств, сначала нажмите любую кнопку
пульта, чтобы Zigbee2MQTT обнаружил его action-триггеры. В отличие от старого
варианта с `sensor.*_action`, этот blueprint не требует включать deprecated
`legacy_action_sensor`.

Параметр «Защита после удержания» нужен для редкого случая, когда после
удержания Zigbee2MQTT дополнительно публикует `on` или `off`. Значение 1000 ms
подходит как начальное.

Список action-значений STYRBAR соответствует документации Zigbee2MQTT:
https://www.zigbee2mqtt.io/devices/E2001_E2002.html
