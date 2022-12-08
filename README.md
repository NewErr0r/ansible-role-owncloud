<h1 align='center'>Автоматизация развёртывания платформы для синхронизации данных и совместной работы с файлами "OwnCloud"</h1>

<p>
    <strong>Шаг 1. </strong> Создание playbook для запуска роли
</p>
<p><i>Пример:</i></p>

    ---
    - name: Deploy OwnCloud
      hosts: all 
      become: true 

      roles: 
        - ansible-role-owncloud

<p>
    <strong>Шаг 2. </strong> Склонировать роль в дирректорию с playbook:
</p>

  <pre>git clone https://github.com/NewErr0r/ansible-role-owncloud.git</pre>

<p>

<p>
    <strong>Пример переопределяемых переменных для playbook. </strong>
</p>
<pre>

#System preparation 
timezone: "Europe/Moscow"

#MariaDB
mariadb_root_password: "P@ssw0rd"
database_name: "owncloud"
database_username: "owncloud"
database_username_password: "owncloud"

#New versions of owncloud work over https — for the protocol to work correctly, we create a virtual domain and configure it to work with the cloud service in NGINX:
server_name: "owncloud.champ.first"

path_owncloud: /var/www/owncloud
path_ssl_cert: /etc/nginx/ssl

ssl_certificate: "{{ path_ssl_cert }}/cert.pem"
ssl_certificate_key: "{{ path_ssl_cert }}/cert.key"

#openssl req -new -x509 -days 1461 -nodes -out cert.pem -keyout cert.key -subj "/C=RU/ST=SPb/L=SPb/O=Global Security/OU=IT Department/CN=owncloud.champ.first/CN=owncloud"
C: "RU"
ST: "SPb"
L: "SPb"
O: "Global"
Security_OU: "IT"
Department_CN: "{{ server_name }}"
CN: "owncloud"

#Download OnwCloud 
url_download_owncloud: "https://download.owncloud.com/server/stable/owncloud-complete-latest.tar.bz2"
dir_download: /tmp
</pre>

<p>
    <strong>Шаг 3. </strong> Запуск playbook:
</p>

  <pre>ansible-playbook -i inventory/owncloud/hosts playbook.yml</pre>

<p>
