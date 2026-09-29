# IKEA BILRESA Matter blueprints

## Blueprint

Файл blueprint:

`blueprints/automation/ikea/bilresa_scrollwheel_last_target.yaml`

Он использует автоматическое сопоставление событий BILRESA:

| Канал | Ярче | Темнее | Кнопка |
|---|---|---|---|
| 1 | Button (1) | Button (2) | Button (3) |
| 2 | Button (4) | Button (5) | Button (6) |
| 3 | Button (7) | Button (8) | Button (9) |

## Helpers

Для памяти последнего выбранного типа нажатия нужны три `input_text` helper’а.
Готовая конфигурация находится в `packages/bilresa_helpers.yaml`.

Если в Home Assistant включены packages, добавьте в `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Если раздел `homeassistant:` уже есть, добавьте только параметр `packages` в него.
После изменения перезапустите Home Assistant.

Альтернатива — создать три helper’а через Settings → Devices & services → Helpers → Create helper → Text:

- `bilresa_channel_1_last`
- `bilresa_channel_2_last`
- `bilresa_channel_3_last`

## Логика

- Одинарное, двойное и тройное нажатие переключают свои наборы ламп/розеток.
- Последний тип нажатия сохраняется в helper канала.
- Длинное нажатие сразу выключает все три набора канала параллельно.
- Button (1/4/7) увеличивает яркость, Button (2/5/8) уменьшает её.
- Колесо работает только с включёнными сущностями домена `light`; умные розетки участвуют в toggle/off, но яркость им не применяется.
- Начальный шаг яркости — 10%; его можно изменить в настройках automation.
