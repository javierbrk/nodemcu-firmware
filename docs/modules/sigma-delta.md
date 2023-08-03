# Sigma-delta Module
| Since  | Origin / Contributor  | Maintainer  | Source  |
| :----- | :-------------------- | :---------- | :------ |
| 2017-01-13 | [Arnim Läuger](https://github.com/devsaurus) | [Arnim Läuger](https://github.com/devsaurus) | [sigma_delta.c](../../components/modules/sigma_delta.c)|

This module provides access to the [sigma-delta](https://en.wikipedia.org/wiki/Delta-sigma_modulation) component. It's a hardware signal generator that can be routed to any of the output GPIOs.

The signal generation is controlled by the [`setprescale()`](#sigma_deltasetprescale) and [`setduty()`](#sigma_deltasetduty) functions.


## sigma_delta.config()
Configures and initializes the sigma delta module. This function has to be called first. 

uint8_t platform_sigma_delta_config( uint8_t channel, uint8_t gpio_num ,uint8_t prescale,uint8_t  duty);

#### Syntax
`sigma_delta.config(channel, pin, prescale, duty)`

#### Parameters
- `channel` 0~7, sigma-delta channel index
- `pin` IO index, see [GPIO Overview](gpio.md#gpio-overview)
- `prescale` prescale 1 to 255
- `duty` -128 to 127

#### Returns
`nil`

#### Example
```lua
sigma_delta.config (0, 4, 0, 128)
sigma_delta.setduty(0, 0)
value = -127
function changeduty()
    print("seteando duty a ", value)
    sigma_delta.setduty(0, value)
    value = value + 10
    if value > 127 then
        value = -127
    end
end
send_data_timer = tmr.create()
send_data_timer:register(3000, tmr.ALARM_AUTO, changeduty)
send_data_timer:start()
```


## sigma_delta.close()
Reenables GPIO functionality at the related pin.

#### Syntax
`sigma_delta.close(channel)`

#### Parameters
- `channel` 0~7, sigma-delta channel index

#### Returns
`nil`

## sigma_delta.setprescale()
Sets the prescale value.

#### Syntax
`sigma_delta.setprescale(channel, value)`

#### Parameters
- `channel` 0~7, sigma-delta channel index
- `value` prescale 1 to 255

#### Returns
`nil`

#### See also
[`sigma_delta.setduty()`](#sigma_deltasetduty)

## sigma_delta.setduty()
Sets the duty value.

#### Syntax
`sigma_delta.setduty(channel, value)`

#### Parameters
- `channel` 0~7, sigma-delta channel index
- `value` duty -128 to 127

#### Returns
`nil`

#### See also
[`sigma_delta.setprescale()`](#sigma_deltasetprescale)

## sigma_delta.setup()
Routes the sigma-delta channel to the specified pin. Target prescale and duty values should be applied prior to enabling the output with this command.

#### Syntax
`sigma_delta.setup(channel, pin)`

#### Parameters
- `channel` 0~7, sigma-delta channel index
- `pin` IO index, see [GPIO Overview](gpio.md#gpio-overview)

#### Returns
`nil`

#### Example
```lua
sigma_delta.setprescale(0, 128)
sigma_delta.setduty(0, 0)
sigma_delta.setup(0, 4)
```
