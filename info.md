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

```cmd
curl -o wazuh-agent-4.14.7-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.14.7-1.x86_64.rpm && sudo WAZUH_MANAGER='192.168.0.100' WAZUH_AGENT_NAME='Redos' rpm -ihv wazuh-agent-4.14.7-1.x86_64.rpm
```

<img width="726" height="250" alt="изображение" src="https://github.com/user-attachments/assets/2e517d7c-c76e-4cc1-924f-3b4023c1e5e0" />

Запуск агента:

```cmd
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

<img width="727" height="262" alt="изображение" src="https://github.com/user-attachments/assets/fc4724d0-051c-4613-b100-ed9218e21df5" />
