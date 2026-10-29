# ansible1

Tere! Minu Ansible'i repo asub siin.

Aine **Skriptimisvahendid** – "Halduse automatiseerimine", Ansible praktikumi 1. osa (Linux-serverid).
Autor: Kerdon Laubert

## Keskkond

| Roll | VM | Hostinimi | OS | IP |
|------|----|-----------|----|----|
| Ansible-server | ansdeb | kerdon | Debian 13 | 192.168.37.193 |
| server/klient1 | ansklient | anskerdon | Debian 13 | 192.168.37.191 |
| server/klient2 | anscentOS | localhost.localdomain | CentOS Stream 9 | 192.168.34.209 |

Ansible: `ansible [core 2.19.11]`, ühendus SSH võtmega `root` kasutajana.

## Struktuur

| Kaust | Lab | Failid | Asukoht Ansible-serveris |
|-------|-----|--------|--------------------------|
| `lab1/` | Lab 1: Ansible automatiseerimislahendus | `ansible.cfg`, `hosts` | `/root/.ansible.cfg`, `/root/hosts` |
| `lab2/` | Lab 2: Apache2 paigaldamine ja userdir | `apache2.yml`, `unikaalne_leht.yml` (iseseisev ülesanne) | `/root/playbooks/` |
| `lab3/` | Lab 3: PHP tugi | `php.yml` (+ iseseisev 1: `phpinfo.php`/`info.php`), `uninst_php.yml` (iseseisev 2) | `/etc/ansible/playbooks/` |
| `lab4/` | Lab 4: MySQL andmebaas | `mysql-server.yml`, `mysql-user.yml` (iseseisev, hindeline) | `/root/playbooks/` |

## Kasutamine

```bash
# Lab 1 – ühenduse kontroll
ansible all -m ping
ansible webservers -m ping
ansible klient1 -m ping

# Lab 2 – Apache2 + userdir (http://<klient>/~user/) ja unikaalne avaleht
ansible-playbook --check /root/playbooks/apache2.yml
ansible-playbook /root/playbooks/apache2.yml
ansible-playbook /root/playbooks/unikaalne_leht.yml

# Lab 3 – PHP (http://<klient>/phpinfo.php), eemaldamine ja taastamine
ansible-playbook /etc/ansible/playbooks/php.yml
ansible-playbook /etc/ansible/playbooks/uninst_php.yml
ansible-playbook /etc/ansible/playbooks/php.yml

# Lab 4 – MySQL 8.4 ametlikust APT hoidlast + root parool ja /root/.my.cnf
ansible-playbook /root/playbooks/mysql-server.yml
ansible-playbook /root/playbooks/mysql-user.yml   # küsib MySQL root parooli
```

## Märkused

- Playbookid toetavad nii Debiani (`apt`, `apache2`) kui CentOS-i (`dnf`, `httpd`, firewalld, SELinux) klienti.
- MySQL APT hoidla on ainult Debianile, seega Lab 4 playbookid jätavad CentOS kliendi vahele (`meta: end_host`).
- `mysql-user.yml` küsib parooli käivitamisel (`vars_prompt`), et parool ei oleks GitHubis avalikult kirjas.
  Pärast käivitamist ei saa MySQL-i enam ilma paroolita sisse (`mysql --no-defaults -u root` → `Access denied`),
  `mysql` käsk töötab root kasutajal faili `/root/.my.cnf` abil.
