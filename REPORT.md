# Lab 1 Report — File Manager
Name: Meshcheryakov Roman
Group: ID24-2
OS: Linux Mint v22
Date: 19.02.2026

## Install and run commands
python3 -m venv .venv
source .venv/bin/activate
python main.py

## Test scenario (commands + outputs)
meshcher@meshcher-VirtualBox:~/projects/file_manager_project$ python main.py
FileManager started. Type 'help' for commands.

fm> mkd docs

OK: created folder 'docs'

fm> new docs/a.txt

OK: created file 'docs/a.txt'

fm> put docs/a.txt "hello world"

OK: wrote 11 chars to 'docs/a.txt'

fm> read docs/a.txt

hello world

fm> show

{'cwd': '/', 'dirs': ['docs'], 'files': []}

fm> in docs

OK: entered 'docs'

fm> where

/docs

fm> out

OK: moved up one level

fm> ren docs/a.txt docs/b.txt

OK: renamed 'docs/a.txt' -> 'docs/b.txt'

fm> dup docs/b.txt docs/copy.txt

OK: copied 'docs/b.txt' -> 'docs/copy.txt'

fm> move docs/copy.txt moved.txt

OK: moved 'docs/copy.txt' -> 'moved.txt'

fm> del moved.txt

OK: removed file 'moved.txt'



## Security check
fm> in ..

ERROR: Forbidden: attempted to escape workspace via '..'

fm> quit

QUIT

meshcher@meshcher-VirtualBox:~/projects/file_manager_project$ exit

exit

(.venv) meshcher@meshcher-VirtualBox:~/projects/file_manager_project$ 

## Conclusion
Я успешно реализовал базовый функционал консольного файлового менеджера. Архитектура разбита на модули, конфигурация загружается из JSON, а встроенная песочница надежно блокирует любые попытки выхода за пределы рабочей папки (workspace). Все операции с файлами и папками отработали корректно.

## Home task
(if assigned, describe what you implemented and show results)
В рамках обязательной части домашнего задания проведено 5 дополнительных тестов:

1. **Использование вложенного пути:** `mkd a/b`
   *Результат:* Ошибка `FileNotFoundError`. 
   *Решение:* Запрещено (оставлено поведение по умолчанию). Приложение не создает родительские папки автоматически при вызове `mkd`.

2. **Удаление непустой папки:** `rmd test_dir` (предварительно добавив туда файл).
   *Результат:* Команда явно завершается ошибкой `OSError: [Errno 39] Directory not empty`. Защита от случайного удаления данных работает.

3. **Чтение отсутствующего файла:** `read ghost.txt`
   *Результат:* Понятная ошибка `ERROR: file not found`. Программа не падает, а продолжает работу.

4. **Переименование в уже существующее имя:** `ren file1.txt file2.txt` (где оба файла существуют).
   *Результат:* Наблюдаемое поведение в Linux Mint — файл `file1.txt` переименовывается, а старый `file2.txt` тихо перезаписывается (заменяется). 

5. **Попытка выйти из workspace через вложенный путь:** `in ../..`
   *Результат:* Заблокировано. Выводится ошибка `ERROR: Forbidden: attempted to escape workspace via '../..'`.

meshcher@meshcher-VirtualBox:~/projects/file_manager_project$ python3 main.py

FileManager started. Type 'help' for commands.

fm> mkd a/b

ERROR: file not found: [Errno 2] No such file or directory: '/home/meshcher/projects/file_manager_project/workspace/a/b'

fm> mkd test_dir

OK: created folder 'test_dir'

fm> new test_dir/temp.txt

OK: created file 'test_dir/temp.txt'

fm> rmd test_dir

ERROR: OSError [Errno 39] Directory not empty: '/home/meshcher/projects/file_manager_project/workspace/test_dir'

fm> read ghost.txt

ERROR: file not found: [Errno 2] No such file or directory: '/home/meshcher/projects/file_manager_project/workspace/ghost.txt'

fm> new file1.txt

OK: created file 'file1.txt'

fm> new file2.txt

OK: created file 'file2.txt'

fm> put file1.txt "111"

OK: wrote 3 chars to 'file1.txt'

fm> put file2.txt "222"

OK: wrote 3 chars to 'file2.txt'

fm> ren file1.txt file2.txt

OK: renamed 'file1.txt' -> 'file2.txt'

fm> read file2.txt

111

fm> in ../..

ERROR: Forbidden: attempted to escape workspace via '../..'

fm> quit

QUIT



