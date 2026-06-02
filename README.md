# ci_ansible_roles

Ansible-проект для настройки nginx virtual hosts и проверки кода через GitHub Actions.

## Что делает playbook

- устанавливает nginx на хосты из `inventory.ini`;
- создает директории `/var/www/<site>`;
- генерирует `index.html` для каждого сайта;
- создает конфиги nginx в `/etc/nginx/sites-available`;
- включает сайты через symlink в `/etc/nginx/sites-enabled`;
- перезагружает nginx при изменении конфигов;
- добавляет записи сайтов в локальный `/etc/hosts`.

## CI

В проекте настроены GitHub Actions:

- `.github/workflows/lint.yml` запускает `ansible-lint` при push и pull request в ветку `main`;
- `.github/workflows/devops_course_pipeline.yml` запускает простой тестовый pipeline при push.

## Структура

```text
.github/workflows/          # GitHub Actions workflows
inventory.ini               # список управляемых хостов
playbook.yml                # основной playbook
group_vars/all.yml          # список сайтов
roles/nginx_vhosts/         # роль для nginx virtual hosts
results/                    # результаты тестовых запусков
```

## Переменные

Список сайтов задается в `group_vars/all.yml`:

```yaml
sites:
  - mehmat.ru
  - fizfak.ru
  - etis.com
  - fakesite.com
```

## Запуск

```bash
ansible-playbook -i inventory.ini playbook.yml
```

После запуска сайты должны быть доступны по именам из переменной `sites`.
