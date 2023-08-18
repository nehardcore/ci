# Домашнее задание к занятию 3 «Использование Ansible»

## Подготовка к выполнению

1. Подготовьте в Yandex Cloud три хоста: для `clickhouse`, для `vector` и для `lighthouse`.
2. Репозиторий LightHouse находится [по ссылке](https://github.com/VKCOM/lighthouse).

## Основная часть

1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает LightHouse.
2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
3. Tasks должны: скачать статику LightHouse, установить Nginx или любой другой веб-сервер, настроить его конфиг для открытия LightHouse, запустить веб-сервер.
```yml
- name: Install Nginx
  hosts: lighthouse
  handlers:
    - name: Start-nginx
      become: true
      ansible.builtin.command: nginx
    - name: Reload-nginx
      become: true
      ansible.builtin.command: nginx -s reload
  tasks:
    - name: NGINX | install epel-release
      become: true
      ansible.builtin.yum:
        name: epel-release
        state: present
    - name: NGINX | install nginx
      become: true
      ansible.builtin.yum:
        name: nginx
        state: present
      notify: Start-nginx
    - name: NGINX | create general config
      become: true
      ansible.builtin.template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: "0644"
      notify: Reload-nginx
- name: Install Lighthouse
  hosts: lighthouse
  handlers:
    - name: Reload-nginx
      become: true
      ansible.builtin.command: nginx -s reload
  pre_tasks:
    - name: Lighthouse | install dependencies
      become: true
      ansible.builtin.yum:
        name: git
        state: present
  tasks:
    - name: Lighthouse | copy from git
      become: true
      ansible.builtin.git:
        repo: "{{ lighthouse_vcs }}"
        version: master
        dest: "{{ lighthouse_location_dir }}"
    - name: Lighthouse | create lighthouse config
      become: true
      ansible.builtin.template:
        src: lighthouse.conf.j2
        dest: /etc/nginx/conf.d/default.conf
        mode: "0644"
      notify: Reload-nginx
```
4. Подготовьте свой inventory-файл `prod.yml`.
```yml
---
clickhouse:
  hosts:
    clickhouse-01:
      ansible_host: 51.250.15.102
      ansible_user: centos
      ansible_ssh_private_key_file: ~/.ssh/id_ed25519
vector:
  hosts:
    vector-01:
      ansible_host: 158.160.57.44
      ansible_user: centos
      ansible_ssh_private_key_file: ~/.ssh/id_ed25519 
      vector_config_dir: "{{ ansible_user_dir }}/vector_config"
      vector_config:
lighthouse:
  hosts:
    lighthouse-01:
      ansible_host: 158.160.115.110
      ansible_user: centos
      ansible_ssh_private_key_file: ~/.ssh/id_ed25519
      nginx_user_name: centos
      lighthouse_location_dir: "{{ ansible_user_dir }}/lighthouse"
```
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
> Исправил все, кроме "no-changed-when: Commands should not change things if nothing needs doing."
> Например, что запускается служба nginx, когда она потенциально может быть уже запущена. Это критично?
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
```zsh
cch@MBP-Costas playbook % ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Nginx] *****************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************
ok: [lighthouse-01]

TASK [NGINX | install epel-release] **************************************************************************************
changed: [lighthouse-01]

TASK [NGINX | install nginx] *********************************************************************************************
fatal: [lighthouse-01]: FAILED! => {"changed": false, "msg": "No package matching 'nginx' found available, installed or updated", "rc": 126, "results": ["No package matching 'nginx' found available, installed or updated"]}

PLAY RECAP ***************************************************************************************************************
lighthouse-01              : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```
Это норма, так как --check не применяет изменения, соответсвенно, nginx не был скачан и нечего было запускать.
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
```yml
PLAY RECAP ***************************************************************************************************************
clickhouse-01              : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-01              : ok=11   changed=9    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-01                  : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
```yml
PLAY RECAP ***************************************************************************************************************
clickhouse-01              : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-01              : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-01                  : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
9. Подготовьте README.md-файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
> https://github.com/nehardcore/ansible/blob/main/08-ansible-03-yandex/playbook/README.md
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-03-yandex` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
