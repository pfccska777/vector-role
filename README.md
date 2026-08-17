# vector-role


Ansible role для установки и настройки Vector.


## Requirements


- Linux x86_64
- systemd
- Ansible


## Role Variables


Переменные, доступные для переопределения:


```yaml
vector_version: "0.57.0"
vector_config_dir: "/etc/vector"
vector_data_dir: "/var/lib/vector"
Example Playbook
---
- name: Install Vector
  hosts: vector
  become: true
  roles:
    - vector
License

MIT



---


# 11. Проверяем роль локально


Пока ещё не надо пихать её в `requirements.yml`.


Для проверки можно создать временный playbook:


```bash
nano test.yml
---
- name: Install Vector
  hosts: vector
  become: true
  roles:
    - vector-role

Например inventory:

---
vector:
  hosts:
    vector-01:
      ansible_host: 192.168.1.20
      ansible_user: user

Запуск:

ansible-playbook -i inventory/prod.yml test.yml
Проверяем на сервере

После выполнения:

systemctl status vector

Должно быть:

Active: active (running)

Проверяем бинарник:

/opt/vector/bin/vector --version

И логи:

journalctl -u vector -f

Там должны начать появляться тестовые события от нашего:

sources:
  demo_logs:
  
  
**Очень важная проверка — идемпотентность**

Запускаем роль второй раз:

ansible-playbook -i inventory/prod.yml test.yml

В идеале в конце должно получиться что-то вроде:

PLAY RECAP


vector-01:
ok=9
changed=0
unreachable=0
failed=0

Именно changed=0 на втором запуске нам особенно приятно видеть   
