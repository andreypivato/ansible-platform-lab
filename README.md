# Platform Engineering Lab — Ansible Multi-Role Infrastructure

Laboratório prático de **Infrastructure as Code (IaC)** e **Configuration Management** utilizando **Ansible** sobre **Ubuntu Server 26.04.1 LTS** em ambiente virtualizado (**VMware Workstation**).

## Arquitetura e Topologia

- **Control Node (`control-node`)**: Executa os playbooks Ansible, gerencia o inventário e orquestra os hosts via SSH (chave `Ed25519`).
- **Web Server (`web-server-01`)**: Grupo `[webservers]` — recebe as roles `webserver` (`open-vm-tools`, `nginx` e página HTML dinâmica via template Jinja2) e `firewall` (`ufw`).
- **Database Server (`db-server-01`)**: Grupo `[dbservers]` — recebe as roles `database` (`postgresql`, `postgresql-contrib`, serviço ativo e criação idempotente de usuário e banco de dados) e `firewall` (`ufw`).

## Hardening e Segmentação com UFW (`roles/firewall`)

- **SSH (`22/tcp`)**: Liberado **apenas** a partir do IP do `control-node`.
- **HTTP (`80/tcp`)**: Liberado nos hosts do grupo `[webservers]`.
- **PostgreSQL (`5432/tcp`)**: Liberado no grupo `[dbservers]` **apenas** a partir do IP do `[webservers]`.
- **Política padrão**: `deny` para entrada (`incoming`) e `allow` para saída (`outgoing`).

## Estrutura do Projeto

```text
.
├── ansible.cfg
├── inventory.ini
├── site.yml
├── .gitignore
├── README.md
└── roles/
    ├── webserver/
    │   ├── tasks/
    │   │   └── main.yml
    │   └── templates/
    │       └── index.html.j2
    ├── database/
    │   ├── tasks/
    │   │   └── main.yml
    │   └── vars/
    │       └── main.yml
    └── firewall/
        ├── tasks/
        │   └── main.yml
        └── vars/
            └── main.yml
