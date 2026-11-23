# Запуск работы

Откройте терминал именно в этой папке. Нужен Python 3.11 или новее.

## Windows PowerShell
```powershell
py -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe -m playwright install chromium
.venv\Scripts\python.exe -m http.server 8000 --bind 127.0.0.1
```
Оставьте сервер работающим. Во втором терминале в этой папке:
```powershell
.venv\Scripts\python.exe -m pytest --collect-only -q
.venv\Scripts\python.exe -m pytest -v --browser chromium --screenshot only-on-failure --tracing retain-on-failure --output test-results --junitxml=report.xml
```

## Linux и macOS
```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m playwright install chromium
.venv/bin/python -m http.server 8000 --bind 127.0.0.1
```
Во втором терминале:
```bash
.venv/bin/python -m pytest --collect-only -q
.venv/bin/python -m pytest -v --browser chromium --screenshot only-on-failure --tracing retain-on-failure --output test-results --junitxml=report.xml
```
На Linux при нехватке библиотек: `.venv/bin/python -m playwright install --with-deps chromium` (может запросить права администратора).

После работы остановите сервер Ctrl+C. Не запускайте команды сразу в двух копиях проекта: обе используют порт 8000.

## Эксперимент с дефектом
1. Сохраните правильный delivery.html.
2. Замените только `value >= 10000` на `value > 10000`.
3. Запустите ту же команду pytest, сохраните XML и test-results.
4. Ожидаются 2 failed и 10 passed — сумма 10000 для city и region.
5. Верните `>=`, повторите прогон: ожидаются 12 passed.
Не меняйте ожидаемые значения тестов.

Trace Viewer: `.venv/bin/python -m playwright show-trace путь/к/trace.zip` (в Windows используйте свой путь к python.exe).

requirements.txt закрепляет основные версии; requirements-observed.txt содержит полный список пакетов фактической проверочной среды, включая вспомогательный PyYAML. Ваша версия Python может отличаться от среды проверки.
