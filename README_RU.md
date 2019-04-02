
http://cxem.net/calc/ledcalc.php

Заменил в pins_RAMPS_FD_V1.h
```cgo
#define HEATER_BED_PIN      8
// на
#define HEATER_BED_PIN      7
```

Открываю в блокноте Configuration_adv.h
Открываю #define HEPHESTOS2_HEATED_BED_KIT
```cgo
#if ENABLED(HEPHESTOS2_HEATED_BED_KIT)
  #undef TEMP_SENSOR_BED
  #define TEMP_SENSOR_BED 70
  #define HEATER_BED_INVERTING true

#endif

// для RAMPS-FD очень важно а то неотключает питание с сопла
#define HEATER_0_INVERTING true
//  За одно и остальные что б не светились лампочки.
#define HEATER_1_INVERTING true
#define HEATER_2_INVERTING true
```

## Калибровать PID нагрева хотэнда и стола (выравнивание графика температуры).

Для этого я использую Printrun (бывший Pronterface). Вводим команду **“M303 E0 C8 S240“** где 

  - M303 – команда калибровки,
  - E0 – хотэнд,
  - C8 – количество циклов нагрева-охлаждения,
  - S230 – температура (максимальная из параметра define HEATER_0_MAXTEMP 230).

Последние результаты записываем в прошивку.

#define  DEFAULT_Kp 12.22
#define  DEFAULT_Ki 0.58
#define  DEFAULT_Kd 64.08


echo:Unknown command: "M1M105"

M502 - сбросить на заводские настройки

После перепрошивки стоит инициализировать EEPROM дефолтными значениями.
Для Этого сначала даем команду M502 а затем M500
M500 для сохранения значений в EEPROM.
M501 выводит текущие параметры.
M502 сбрасывает параметры в дефолтные (но только в оперативной памяти, их все еще нужно сохранить в EEPROM).

Работа с EEPROM проста. Выводим список параметров командой M501. Далее правим нужные параметры соответствующими командами из выведенного списка (например для ускорений M201 X1500 Y1500 Z200 E9000). Сохраняем параметры в EEPROM командой M500.