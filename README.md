Vector-role
=========

Роль для установки и настройки vector. Реализована поддержка deb & rpm based дистрибутивов. Минимальный конфиг vector собирается из template. По умолчанию запускается установка vector и конфигурирование из template. Это поведении можно изменить указав тэг при запуске. Также есть возможность удалить vector и конфигурацию при использованиии тэга.

Минимальная конфигурация vector представляет из себя чтение файла лога приложения и добавления записей в СУБД Clickhouse. Для подключения к СУБД используются `basic` аутентификация. Пароль можно определить в default, vars или extra_vars. Лучше с использовать ansible-vault, пример ниже. Для чуствительных данных используется каталог vault. 

Requirements
------------

Для использования потребуются следующие модули:
  - ansible.builtin.package
  - ansible.builtin.file
  - ansible.builtin.template
  - ansible.builtin.meta
  - ansible.builtin.apt
  - ansible.builtin.get_url
  - ansible.builtin.dnf

Role Variables
--------------

| Name | Description | Type | Default |
|------|-------------|------|---------|
| <a name="input_vector_version"></a> [vector\_version](#input\_vector\_version) | Версия vector для установки | `"string"` | `0.51.1` |
| <a name="input_app_log_path"></a> [app\_log\_path](#input\_app\_log\_path) | Путь до лог файла приложения | `string` | `/tmp/for_vector_test.log` |
| <a name="input_db_user"></a> [db\_user](#input\_db\_user) | Пользователь СУБД Clickhouse для подключения | `string` | `null` |
| <a name="input_db_password"></a> [db\_password](#input\_db\_password) | Пароль пользователя СУБД Clickhouse для подключения. | `string` | `null` |
| <a name="input_db_name"></a> [db\_name](#input\_db\_name) | Имя БД в Clickhouse | `string` | `defaults` |
| <a name="input_db_table"></a> [db\_table](#input\_db\_table) | Имя таблицы в Clickhouse | `string` | `example` |
| <a name="input_db_host"></a> [db\_host](#input\_db\_host) | Имя или ip-адрес хоста с Clickhouse.  Используется есть не задана группа clickhouse в inventory | `string` | `localhost` |
| <a name="input_db_port"></a> [db\_port](#input\_db\_port) | Порт подключения к СУБД Clickhouse | `number` | `8123` |
| <a name="input_db_proto"></a> [db\_proto](#input\_db\_proto) | Протокол взаимодействия с СУБД Clickhouse | `string` | `http` |
| <a name="input_db_healthcheck"></a> [db\_healthcheck](#input\_db\_healthcheck) | Выполнять healthceck СУБД или нет. Устанавливает значение в конифгурации vector (healthcheck.enabled) | `boot` | `true` |

Example Playbook
----------------

Переменные можно переопределить в group_vars или указать в play.

Пример playbook:

    - hosts: clickhouse
      become: true
      roles:
         - clickhouse-role

    - hosts: vector
      become: true
      roles:
         - vector-role

  Перед запуском рекомендуется зашифровать чуствительные данные при помощи ansible-vault:

     ansible-vault encrypt ./vault/secrets.yml

  Пример запуска ansible-playbook:

  **Запуск установки и настройки vector:**
  
    ansible-playbook -i hosts.ini site.yml --ask-vault-pass

  **Запуск только установки vector:**
  
    ansible-playbook -i hosts.ini site.yml --tags vector_install

  **Запуск только настройки vector:**
  
    ansible-playbook -i hosts.ini site.yml --ask-vault-pass --tags vector_config
  
  **Удание vector и конфигурации:**
  
    ansible-playbook -i hosts.ini site.yml --ask-vault-pass --tags vector_remove


License
-------

BSD

Author Information
------------------

Perman Maxim
maxim@7perman.ru
