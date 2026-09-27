sudo grep -i -E 'vulnerability-scanner|vulnerability|001' /var/ossec/logs/ossec.log | tail -50

sudo grep -i 'openssl' /var/ossec/logs/ossec.log | tail -20

## Подготовка и настройка Wazuh

- [Официальный образ wazuh](https://packages.wazuh.com/4.x/vm/wazuh-4.14.7.ova)

Чтобы установить **Wazuh** в виртуальном окружении достаточно просто скопировать файл `.ova` в **VMware**. В данном проекте **Wazuh** был установлен на домашнем ПК, а 
**РЕД ОС** на ноутбуке в той же локальной сети. Для того чтобы виртуальные машины смогли обнаружить друг друга, будет использован сетевой адаптер `Bridged`:

<img width="296" height="428" alt="изображение" src="https://github.com/user-attachments/assets/7f7a9e4d-a5e4-4c7c-b8dd-c48c2e1eed8c" />

<img width="810" height="259" alt="изображение" src="https://github.com/user-attachments/assets/849dbcbb-08c7-4af0-ab6b-5d75abf246c0" />

Вход в web-панель **Wazuh**:

  - логин: `admin`
  - пароль: `admin`

<img width="965" height="935" alt="изображение" src="https://github.com/user-attachments/assets/403cd654-86be-4c60-8471-adcae5a7cb3f" />

Оказавшись на главной странице необходимо нажать на кнопку `Deploy new agent`

<img width="1920" height="928" alt="изображение" src="https://github.com/user-attachments/assets/b71f40f4-adf1-4a3c-9bab-0280d413aaed" />

Далее необходимо заполнить поля:

<img width="1912" height="1837" alt="изображение" src="https://github.com/user-attachments/assets/6dc2a8ae-5be6-4d8f-a199-e2d1eb1a0096" />

## Подготовка и настройка РЕД ОС

- [Официальный образ РЕД ОС](https://files.red-soft.ru/redos/8.0/x86_64/iso/redos-8-20260716.0-Everything-x86_64-DVD1.iso)

При установке **РЕД ОС** можно воспользоваться инструкцией с официального сайта - [Инструкция для установки РЕД ОС](https://redos.red-soft.ru/base/redos-8_0/8_0-install/8_0-install-red-os/#start)

Сетевой адаптер также используется `Bridged`:

<img width="302" height="392" alt="изображение" src="https://github.com/user-attachments/assets/aecfc2d9-b472-4c97-894f-0bfc5450f8ee" />

<img width="728" height="348" alt="изображение" src="https://github.com/user-attachments/assets/80ada8ad-b273-4e22-8820-af98ff739512" />

Для установки агента на **РЕД ОС** используется команда выданная web-панелью:

```bash
curl -o wazuh-agent-4.14.7-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.14.7-1.x86_64.rpm && sudo WAZUH_MANAGER='192.168.0.100' WAZUH_AGENT_NAME='Redos' rpm -ihv wazuh-agent-4.14.7-1.x86_64.rpm
```

<img width="726" height="250" alt="изображение" src="https://github.com/user-attachments/assets/2e517d7c-c76e-4cc1-924f-3b4023c1e5e0" />

Запуск агента:

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

<img width="727" height="262" alt="изображение" src="https://github.com/user-attachments/assets/fc4724d0-051c-4613-b100-ed9218e21df5" />

После установки в списке агентов появилось новое подключение:

<img width="1912" height="1160" alt="изображение" src="https://github.com/user-attachments/assets/576a4c68-f0a9-4568-b7d7-ffeeb828ab5e" />

Перейдя по ссылке `active` можно попасть на подробный список агентов с описанием каждого:

<img width="1920" height="898" alt="изображение" src="https://github.com/user-attachments/assets/f6e0198f-a6e6-439d-a128-af2807049e5e" />

## Проверка модулей

На **Агенте** нужно проверить что система верно установилась и сервер может собирать информацию об ОС. Для этого нужно проверить блок `syscollector`:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

<img width="436" height="425" alt="изображение" src="https://github.com/user-attachments/assets/a08c7d3b-7ad2-42ee-a65d-7f9d4e69655d" />

На **Сервере** (в том же файле, что и на агенте) необходимо проверить что в блоке `<vulnerability-detector>` стоит параметр `yes`:

<img width="416" height="93" alt="изображение" src="https://github.com/user-attachments/assets/a3f286eb-82c7-4382-a4de-75a48f497087" />

В web-панели сервера теперь можно ознакомится с полной информацией об агенте:

<img width="1914" height="1271" alt="изображение" src="https://github.com/user-attachments/assets/21c53287-aabf-4ed0-ab01-a0550ceb31a9" />

## Инвентаризация

На странице `IT Hygiene` можно ознакомится с более подробным описанием системы, значит сервер успешно работает и поддерживает сбор RPM-пакетов:

<img width="1915" height="1039" alt="изображение" src="https://github.com/user-attachments/assets/23ed5190-910d-4936-bc9d-fdeba314451c" />

## Проверка соответствия требованиям безопасности

Для проверки конфигурации **РЕД ОС** был использован модуль **Security Configuration Assessment (SCA)**. Данный модуль позволяет проверять 
настройки операционной системы на соответствие заданным политикам безопасности.

На агенте РЕД ОС в файле `/var/ossec/etc/ossec.conf` был проверен и настроен блок **<sca>**:

```bash
<sca>
    <enabled>yes</enabled>
    <scan_on_start>yes</scan_on_start>
    <interval>1m</interval>
    <skip_nfs>yes</skip_nfs>
</sca>
```

При проверке стандартных политик было обнаружено, что имеющаяся политика `cis_rhel10_linux.yml` предназначена для **RHEL 10**. Поскольку на агенте используется РЕД ОС 8.0.3, данная политика не применяется:

<img width="514" height="635" alt="изображение" src="https://github.com/user-attachments/assets/5010b2af-ab62-4592-aae1-2f3ac0be6f44" />

<img width="1117" height="363" alt="изображение" src="https://github.com/user-attachments/assets/312b2f25-5709-4b2b-a379-3f0fcd2404ae" />

Для проведения проверки была создана собственная SCA-политика, учитывающая конфигурацию **РЕД ОС**:

<img width="871" height="603" alt="изображение" src="https://github.com/user-attachments/assets/7ff694b5-1fe2-4843-b7e6-18906bf46773" />

  - В политике были заданы проверки различных параметров безопасности системы, в том числе состояния firewalld, SSH, системных файлов и наличия небезопасных сервисов.

Пользовательская политика была подключена в конфигурации агента: 

<img width="759" height="188" alt="изображение" src="https://github.com/user-attachments/assets/c3480762-5569-4d5d-8835-cfb550aa9690" />

После выполнения проверки результаты стали доступны в веб-панели **Wazuh**. Подробнее с ней можно ознакомится перейдя в `Configuration Assessment` на главной странице:

<img width="1923" height="898" alt="изображение" src="https://github.com/user-attachments/assets/7bf7f80f-5f62-443a-94dd-a79f3b64f9ae" />

Единственная непройденная проверка связана с состоянием firewalld, поскольку данный сервис был отключён.
