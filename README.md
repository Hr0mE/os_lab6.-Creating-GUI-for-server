# Temperature Monitor GUI (Lab 6)

GUI-клиент для визуализации температурных данных. Реализован на C с использованием GTK3. Получает данные по HTTP от сервера из лабораторной работы №5 и отображает их в числовом и графическом виде.

## Что делает приложение

* Показывает **текущую температуру** и время последнего обновления
* Строит графики:

  * почасовые значения за последние 24 часа (линейный)
  * средние значения по дням за месяц (столбчатый)
* Обновляет данные автоматически (раз в 30 секунд)
* Позволяет обновить данные вручную

## Зависимости

Необходимые библиотеки и инструменты:

* GTK+ 3
* libcurl
* json-c
* CMake ≥ 3.10
* компилятор C (GCC / Clang)

### Установка

**Ubuntu / Debian**

```bash
sudo apt-get update
sudo apt-get install -y \
  build-essential \
  cmake \
  libgtk-3-dev \
  libcurl4-openssl-dev \
  libjson-c-dev
```

**macOS**

```bash
brew install gtk+3 curl json-c pkg-config
```

**Windows (MSYS2 / MinGW)**

```bash
pacman -S mingw-w64-x86_64-gtk3 \
          mingw-w64-x86_64-curl \
          mingw-w64-x86_64-json-c \
          mingw-w64-x86_64-cmake \
          mingw-w64-x86_64-gcc
```

## Сборка

**Linux / macOS**

```bash
sh scripts/build_linux.sh
```

**Windows**

```cmd
cd scripts
build_windows.cmd
```

## Запуск

### 1. Сервер (Lab 5)

GUI не работает автономно — сначала необходимо запустить HTTP-сервер:

```bash
cd labs/5
sh scripts/generate_data.sh
sh scripts/build_linux.sh
./build/bin/temp_monitor --http 8080
```

### 2. GUI-клиент

```bash
cd labs/6
./build/bin/temp_monitor_gui
```

По умолчанию приложение ожидает сервер на `localhost:8080`.

## Используемое API

GUI обращается к следующим endpoint’ам сервера:

* `GET /api/current` — текущее значение температуры
* `GET /api/hourly` — почасовые данные
* `GET /api/daily` — дневные средние значения

## Технические детали

* HTTP-клиент: **libcurl**
* Разбор данных: **json-c**
* Отрисовка графиков: **Cairo (через GTK)**
* Масштабирование графиков выполняется автоматически
* Данные кэшируются между обновлениями

## Отладка

Проверка доступности сервера:

```bash
curl http://localhost:8080/api/current
```

Запуск GUI с инструментами GTK:

```bash
GTK_DEBUG=interactive ./bin/temp_monitor_gui
```
