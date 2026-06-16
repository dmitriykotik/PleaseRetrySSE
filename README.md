# PleaseRetrySSE[^1]

PleaseRetrySSE[^1] - это плагин-перехватчик Game Coordinator сообщений для
Counter-Strike: Global Offensive (CS:GO), написанный на C++ с
использованием SSE_PDK (SmartSteamEmu Plugin Development Kit).

Плагин предназначен для анализа, логирования и имитации GC-трафика при
использовании SmartSteamEmu.

## Установка

1.  Выберите нужную ревизию плагина:

    -   [x86](https://github.com/dmitriykotik/PleaseRetry/releases/download/1.0.0.56/PleaseRetry.dll)
    -   [x64](https://github.com/dmitriykotik/PleaseRetry/releases/download/1.0.0.56/PleaseRetry64.dll)

2.  Поместите `PleaseRetry.dll` или `PleaseRetry64.dll` в папку загрузки плагинов SmartSteamEmu.

3.  Создайте файл конфигурации `pleaseretry.ini` в корневой папке игры
    --- там, где находится `csgo.exe`.

## Конфигурация (pleaseretry.ini)

Пример:

``` ini
ip=127.0.0.1
port=27010
ranges=2070-2099,9000-9999
ids=2100,2005
```

Параметры:

-   `ip` --- IP-адрес TCP-сервера
-   `port` --- порт TCP-сервера
-   `ranges` --- диапазоны ID сообщений (формат start-end, через
    запятую)
-   `ids` --- конкретные ID сообщений (через запятую)

## Принцип работы

-   Плагин оборачивает интерфейс `ISteamGameCoordinator001`.
-   Перехватывает `SendMessage` и `RetrieveMessage`.
-   Если ID сообщения попадает в указанные `ids` или `ranges`,
    сообщение:
    -   отправляется на внешний TCP-сервер;
    -   может быть заменено или дополнено ответом от сервера.
-   Входящие сообщения от сервера помещаются во внутреннюю очередь и
    возвращаются клиенту как GC-сообщения.

## Формат TCP-сообщений

Сервер должен читать и отправлять данные в следующем формате:
```cplusplus
    uint32 msgType
    uint32 msgSize
    uint8  payload[msgSize]
```
Передача выполняется без шифрования и без дополнительной сериализации
(raw uint32).

## Сборка

-   Тип проекта: DLL (C++)
-   Компилятор: MSVC (рекомендуется)
-   Необходима линковка `ws2_32.lib`
-   Архитектура сборки должна соответствовать версии SSE (x86/x64)

## Ограничения

-   Нет шифрования трафика
-   Нет проверки корректности входящих данных
-   Использование возможно в основном только в тестовой/локальной среде
-   Требует доработки

## Тестовый сервер
Вы можете использовать [**этот сервер**](https://github.com/dmitriykotik/PleaseRetry-TestServer) для тестирования плагина.

[^1]: PleaseRetrySSE - это плагин, созданный для эмулятора SSE (_сокр. от SmartSteamEmu_). Ранее назывался `PleaseRetry`. На данный момент `PleaseRetry` - это комплекс инструментов для эмуляции Steam*.

`* Steam - это торговая марка принадлежащая Valve Corporation. PleaseRetry, PleaseRetrySSE, PleaseRetry-TestServer и SmartSteamEmu - не имеют никакого отношения к Valve Corporation и Steam.`
