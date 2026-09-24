## Подготовка и настройка Wazuh

- [Официальный образ wazuh](https://packages.wazuh.com/4.x/vm/wazuh-4.14.7.ova)

Чтобы установить **Wazuh** в виртуальном окружении достаточно просто скопировать файл `.ova` в **VMware**. Для того чтобы другие виртуальные машины смогли 
обнаружить **Wazuh**, будет использован сетевой адаптер `NAT`:

<img width="302" height="448" alt="изображение" src="https://github.com/user-attachments/assets/7777ca51-9e7a-4975-a0a8-5508176d0fc5" />

<img width="813" height="243" alt="изображение" src="https://github.com/user-attachments/assets/fe51548b-78de-4493-8325-11420cde23a7" />

Вход в web-панель **Wazuh**:

  - логин: `admin`
  - пароль: `admin`

<img width="965" height="935" alt="изображение" src="https://github.com/user-attachments/assets/403cd654-86be-4c60-8471-adcae5a7cb3f" />

Оказавшись на главной странице необходимо нажать на кнопку `Deploy new agent`

<img width="1920" height="928" alt="изображение" src="https://github.com/user-attachments/assets/b71f40f4-adf1-4a3c-9bab-0280d413aaed" />

Далее необходимо заполнить поля:

<img width="1912" height="1837" alt="изображение" src="https://github.com/user-attachments/assets/aacfcc4d-6e6e-4d39-8c2a-cab458fe0a78" />

## Подготовка и настройка РЕД ОС

- [Официальный образ РЕД ОС](https://files.red-soft.ru/redos/8.0/x86_64/iso/redos-8-20260716.0-Everything-x86_64-DVD1.iso)

При установке **РЕД ОС** можно воспользоваться инструкцией с официального сайта - [Инструкция для установки РЕД ОС](https://redos.red-soft.ru/base/redos-8_0/8_0-install/8_0-install-red-os/#start)

Сетевой адаптер также используется `NAT`:

<img width="298" height="396" alt="изображение" src="https://github.com/user-attachments/assets/22bf47ef-41c4-4369-8c05-400d110a7d59" />

Для установки агента на **РЕД ОС** используется команда выданная web-панелью:

```cmd
curl -o wazuh-agent-4.14.7-1.x86_64.rpm https://packages.wazuh.com/4.x/yum/wazuh-agent-4.14.7-1.x86_64.rpm && sudo WAZUH_MANAGER='192.168.188.132' WAZUH_AGENT_NAME='Redos' rpm -ihv wazuh-agent-4.14.7-1.x86_64.rpm
```

<img width="1276" height="220" alt="изображение" src="https://github.com/user-attachments/assets/b330d42f-35e6-4025-95c1-9a1248e11178" />

Запуск агента:

```cmd
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

<img width="1145" height="106" alt="изображение" src="https://github.com/user-attachments/assets/1ee0ae51-da09-45bc-9a2b-58e890619901" />
