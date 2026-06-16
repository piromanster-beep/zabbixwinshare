# zabbixwinshare
Monitor Windows network backup drives (F:–Y:) with auto-discovery and UNC path verification.

# Zabbix Template: Backup Disks

Мониторинг сетевых дисков (бэкапных шар) на Windows Server 2012+ через Zabbix Agent.

## Возможности

- Автообнаружение подключённых сетевых дисков (буквы F:–Y:)
- Проверка реальной доступности шары (Test-Path через UNC, а не просто буква диска)
- Отображение имени шары в уведомлениях
- Не зависит от сессии пользователя (работает для служб)

## Требования

- Zabbix Server 7.0+
- Zabbix Agent 7.0+ на Windows Server 2012+
- PowerShell 3.0+

## Установка

1. Скопируйте UserParameter'ы из `zabbix_agentd.conf` в конфиг агента.
2. Перезапустите Zabbix Agent.
3. Импортируйте `template_backup_disks.yaml` в Zabbix.
4. Привяжите шаблон к хосту.

## Как работает

- `disk.discover` — находит все сетевые диски с буквами F:–Y: и возвращает JSON для Low-Level Discovery.
- `disk.access[{#DISK}]` — извлекает UNC-путь диска через `net use`, затем проверяет доступность папки через `Test-Path`.
- `disk.unc.path[{#DISK}]` — возвращает имя шары (например, `buhback`) для диагностики.

## Примечания

- Если имена шар содержат юникод, например кирилицу, в Zabbix они могут отображаться как `??`. Не критично для мониторинга.
- Для дисков, подключённых от доменной учётной записи, агент должен работать от того же пользователя (или использоваться `Test-Path` с UNC).
