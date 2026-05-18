# Projeto 01 - DevOps com Vagrant e Ansible

## Informações Gerais

**Disciplina:** Administração de Sistemas Abertos  
**Professor:** Leonidas Lima  
**Período:** 2026.1  
**Instituição:** Instituto Federal da Paraíba - Campus João Pessoa

## Integrante

- **Pedro Weverton Barbosa** - Matrícula: 20222380020

## Descrição

Desenvolvimento de competências práticas em DevOps e Infraestrutura como Código (IaC) utilizando Vagrant e Ansible para provisionar uma infraestrutura virtual completa e automatizar a configuração do sistema operacional e serviços essenciais.

## Infraestrutura

| VM | Hostname | IP | RAM | Serviços |
|----|----------|----|----|---------|
| arq | arq.pedro.barbosa.devops | 192.168.56.20 | 512MB | DHCP, DNS, NFS, LVM |
| db | db.pedro.barbosa.devops | 192.168.56.21 | 512MB | MariaDB, autofs |
| app | app.pedro.barbosa.devops | 192.168.56.22 | 512MB | Apache2, autofs |
| cli | cli.pedro.barbosa.devops | DHCP (50-100) | 1024MB | Firefox, X11, autofs |

## Serviços Configurados

### Todas as VMs
- Sistema atualizado (update e upgrade)
- NTP configurado com pool.ntp.br
- Timezone America/Recife
- Grupo ifpb criado
- Usuários pedro e barbosa
- SSH: autenticação por chave pública, banner, restrições de acesso
- Cliente NFS instalado
- Sudo para grupo ifpb

### Servidor de Arquivos (arq)
- **DHCP Autoritativo:** Escopo 192.168.56.50-100, lease-time 180s, máximo 3600s
- **DNS Autoritativo:** Zonas direta e reversa para pedro.barbosa.devops, forwarders 1.1.1.1 e 8.8.8.8
- **LVM:** VG dados com LV ifpb (15GB ext4) montado em /dados
- **NFS:** Compartilhamento /dados/nfs, sync, all_squash, usuário nfs-ifpb sem shell

### Servidor de Banco de Dados (db)
- MariaDB instalado e habilitado
- Montagem automática NFS em /var/nfs via autofs

### Servidor de Aplicação (app)
- Apache2 com página personalizada (descrição, nome, matrícula)
- Montagem automática NFS em /var/nfs via autofs

### Cliente (cli)
- Firefox ESR e xauth instalados
- X11 Forwarding habilitado no SSH
- Montagem automática NFS em /var/nfs via autofs

## Estrutura do Projeto
projeto-devops/
├── README.md
├── Vagrantfile
└── ansible/
    ├── ansible.cfg
    ├── inventory/
    │   └── hosts.yml
    └── playbooks/
        ├── all.yml
        ├── arq.yml
        ├── db.yml
        ├── app.yml
        ├── cli.yml
        └── templates/
            ├── dhcpd.conf.j2
            ├── named.conf.options.j2
            ├── named.conf.local.j2
            ├── db.dominio.j2
            ├── db.reverso.j2
            └── exports.j2

## Como Executar

### Pré-requisitos
- VirtualBox 6.1+
- Vagrant 2.2+
- Plugin vagrant-vbguest (opcional)
- Ansible 2.9+ (apenas para execução manual)

### Provisionamento Automático

```bash
# Clonar repositório
git clone https://github.com/Pedro-WB/projeto-asa.git
cd projeto-devops

# Iniciar todas as VMs (provisionamento automático)
vagrant up
```
