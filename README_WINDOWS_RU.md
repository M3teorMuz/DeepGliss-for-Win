# DeepGliss — Windows x64 / VST2

Это исходный проект DeepGliss 0.1.7, подготовленный для сборки **64-битного VST2-плагина под Windows** и установки в FL Studio.

## Важное

- Оригинальный проект уже является VST2-плагином: это указано в README автора, а код использует `vst = 0.2.1`.
- Исходник не переписывался на другой плагинный API. Изменена конфигурация GitHub Actions: теперь она собирает Windows x64 и упаковывает DLL под именем `DeepGliss.dll`.
- `DeepGliss.vst.zip`, который был приложен отдельно, — macOS VST bundle. Он не нужен для Windows.
- Плагин VST2, не VST3. Для FL Studio нужна 64-битная версия программы.

## Сборка через GitHub — пошагово

1. Распакуй архив `DeepGliss-Windows-VST2.zip` на компьютере.
2. Создай пустой репозиторий на GitHub. Например, `DeepGliss-Windows`.
3. Загрузи **содержимое** распакованной папки в корень репозитория. В корне должны находиться `Cargo.toml`, папки `src` и `.github`, файл `LICENSE`.
4. Открой вкладку **Actions** репозитория. Если GitHub спросит разрешить Actions, подтверди.
5. В списке workflows выбери **Build DeepGliss VST2 (Windows x64)**.
6. Нажми **Run workflow** → **Run workflow**.
7. Дождись зелёного статуса завершения.
8. Открой завершённый запуск workflow. Внизу страницы в разделе **Artifacts** скачай `DeepGliss-Windows-x64-VST2`.
9. Распакуй скачанный архив. Внутри будет `DeepGliss.dll`.

## Установка в FL Studio

1. Закрой FL Studio.
2. Скопируй `DeepGliss.dll` в стандартную папку VST2:

   `C:\Program Files\Common Files\VST2`

   Если папки `VST2` нет, создай её. Можно использовать и другую папку, если она добавлена в настройках FL Studio.

3. Запусти FL Studio.
4. Открой **Options → Manage plugins**.
5. Убедись, что папка с `DeepGliss.dll` добавлена в **Plugin search paths**.
6. Нажми **Find installed plugins**.
7. Найди DeepGliss в списке найденных плагинов.

## Если FL Studio не видит плагин

- Убедись, что установлена 64-битная FL Studio.
- Проверь, что файл называется именно `DeepGliss.dll`, а не `gliss.dll` и не `DeepGliss.dll.dll`.
- Проверь, что DLL лежит в папке, которую сканирует FL Studio.
- Включи проверку дополнительных/недействительных плагинов в Plugin Manager и посмотри журнал сканирования.
- Не клади DLL из macOS-архива `DeepGliss.vst.zip` в Windows.

## Важное замечание по MIDI

DeepGliss генерирует MIDI pitch-bend-события и предназначен для работы в MIDI-цепочке с инструментом, который принимает pitch bend/MPE. Сам факт появления плагина в списке FL Studio ещё не означает, что MIDI-маршрутизация настроена.

После установки проверь маршрут MIDI: DeepGliss должен получать ноты/аккорды, а принимающий синтезатор — получать выходящие pitch-bend-события. Поведение MIDI-выхода зависит от способа загрузки и маршрутизации VST2 в FL Studio и от поддержки pitch bend принимающим инструментом.

## Что находится в проекте

- `src/` — исходный код DeepGliss.
- `Cargo.toml` — зависимости Rust.
- `.github/workflows/build-windows-vst2.yml` — сборка Windows x64 через GitHub Actions.
- `LICENSE` — лицензия исходного проекта.

Сборка использует Rust nightly, поскольку исходник содержит nightly feature attributes.
