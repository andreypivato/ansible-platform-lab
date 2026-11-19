# Platform Engineering Lab — Ansible Multi-Role Infrastructure

Laboratório prático de **Infrastructure as Code (IaC)** e **Configuration Management** utilizando **Ansible** sobre **Ubuntu Server 26.04.1 LTS** em ambiente virtualizado (**VMware Workstation**).

## Arquitetura e Topologia

- **Control Node (`control-node`)**: Executa os playbooks Ansible, gerencia o inventário e orquestra os hosts via SSH (chave `Ed25519`).
- **Web Server (`web-server-01`)**: Grupo `[webservers]` — recebe a role `webserver` (`open-vm-tools`, `nginx` e deploy de página HTML dinâmica via template Jinja2 usando `ansible_facts`).
- **Database Server (`db-server-01`)**: Grupo `[dbservers]` — recebe a role `database` (`postgresql`, `postgresql-contrib`, serviço habilitado no boot e criação idempotente de usuário e banco de dados da aplicação).

## Estrutura do Projeto

```text
.
├── ansible.cfg
├── inventory.ini
├── site.yml
└── roles/
    ├── webserver/
    │   ├── tasks/
    │   │   └── main.yml
    │   └── templates/
    │       └── index.html.j2
    └── database/
        ├── tasks/
        │   └── main.yml
        └── vars/
            └── main.yml
