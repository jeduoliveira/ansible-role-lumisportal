LumisXP
=========

Instala e configura o [lumisportal](https://lumisxp.lumis.com.br/doc/lumisportal/11.2.0/pt-BR/lumis.installation_and_configuration.system_requirements.html) no RedHat/CentOS 7.

O LumisXP (Lumis Experience Platform) é uma plataforma para a criação e gestão de soluções para a experiência digital do cliente.

Utilizada por grandes empresas dos mais variados segmentos de mercado, a plataforma conta com diversos recursos que ajudam as empresas a acelerar a transformação digital dos seus negócios. As funcionalidades do LumisXP permitem que você desenvolva os canais digitais da sua empresa, colocando o seu cliente como elemento central da solução.

Do ponto de vista de gestão da experiência do cliente, a plataforma permite que você analise com precisão o comportamento dos seus usuários, de forma que seja possível segmentar não só a comunicação, como também os serviços presentes nos seus canais digitais. Além disso, a plataforma dá total liberdade para as equipes de design e front-end e garante mais agilidade aos profissionais de desenvolvimento. O LumisXP garante ainda segurança e alto desempenho nas soluções de digital customer experience.

Todos esses recursos combinados permitem às empresas criar um ciclo constante de melhorias nos seus canais digitais, de forma a garantir experiências personalizadas para os clientes e insights valiosos para a tomada de decisão dos gestores.

Requirements
------------

São necessários os seguintes softwares 

- Zulu OpenJDK. [jeduoliveira.zulu_openjdk](https://galaxy.ansible.com/jeduoliveira/zulu_openjdk)
- Elasticsearch [jeduoliveira.elasticsearch](https://galaxy.ansible.com/jeduoliveira/elasticsearch)
- Tomcat [jeduoliveira.tomcat](https://galaxy.ansible.com/jeduoliveira/tomcat)
- MySQL [geerlingguy.mysql](https://galaxy.ansible.com/geerlingguy/mysql)


Role Variables
--------------
As variáveis abaixo são listadas como padrão. (Veja defaults/main.yml)

    java_web_server: tomcat

    lumis_name: lumisportal
    lumis_directory: /opt

    lumis_user: tomcat
    lumis_group: tomcat

    tomcat_name: tomcat
    tomcat_directory: /opt
    tomcat_context: ROOT

    ##############################################
    # elasticsearch configuration
    ##############################################
    lumis_elasticsearch_cluster_name: lumisportal
    lumis_elasticsearch_http_9200: 127.0.0.1:9200
    lumis_elasticsearch_http_9300: 127.0.0.1:9300

    ##############################################
    # lumis statics files patch
    ##############################################
    lumis_static_path: /opt/htdocs

    ##############################################
    # Database configuration lumishibernate.cfg.xml
    ###############################################

    lumis_db_type: MySQL
    lumis_db_user: lumisportal
    lumis_db_pass: lumis#admin
    lumis_db_url: localhost
    lumis_db_port: 3306
    lumis_db_name: lumisportal
    lumis_db_hikari_minimumIdle: 100
    lumis_db_hikari_maximumPoolSize: 100
    lumis_db_driver_class: com.mysql.cj.jdbc.Driver

    ###############################################
    # Configuration lumisportalconfig.xml
    ###############################################
    lumis_server_id: lumis1

    ###############################################
    # HtmlGeneration
    ###############################################
    lumis_htmlGeneration_enable: true
    lumis_htmlGeneration_connectTimeout: 30000
    lumis_htmlGeneration_pageRequestTimeout: 50000
    lumis_htmlGeneration_frameworkUrl: http://localhost:8080
    lumis_htmlGeneration_htmlCacheLogNavigation: 0
    lumis_htmlGeneration_htmlFileExtension: .htm
    lumis_htmlGeneration_shtmlFileExtension: .shtml
    lumis_htmlGeneration_ssi_sendRedirectOnPageNotFound: true
    lumis_htmlGeneration_ssi_waitBeforeSendRedirect: 500
    lumis_htmlGeneration_expirationLimit: 0
    ################################################
    # ErrorPager
    ################################################
    lumis_errorPage_enable: true
    lumis_errorPage: customizedErrorPage.jsp
    ################################################
    # Cluster
    ################################################
    lumis_cluster_enable: False
    

Dependencies
------------
  
    dependencies:
      - jeduoliveira.zulu_openjdk
      - jeduoliveira.tomcat
      - jeduoliveira.elasticsearch
      - geerlingguy.mysql


Example Playbook
----------------

    ---
    - hosts: all
      gather_facts: yes
      become: yes

      roles:    
        - role: jeduoliveira.zulu_openjdk
        - role: jeduoliveira.elasticsearch
          elasticsearch_version: 6.2.2

        - role: jeduoliveira.tomcat
          tomcat_version: 9
          tomcat_release: 9.0.19
        
        - role: geerlingguy.mysql
          mysql_enabled_on_startup: true
          mysql_root_password_update: true
          mysql_root_password: 123456789
          mysql_databases:
            - name: lumisportal
              encoding: utf8
              collation: utf8_general_ci
         
          mysql_users:
            - name: lumisportal
              host: "%"
              password: 123456789
              priv: "lumisportal.*:ALL"
        
        - role: ansible-role-lumisportal
          lumis_user: tomcat
          lumis_group: tomcat
          lumis_version: 11.2.0
          lumis_release: 190404
          lumis_db_user: lumisportal
          lumis_db_pass: 123456789

License
-------

MIT / BSD

Author Information
------------------

Está role foi criada em 2019 por [Jorge Eduardo](https://www.linkedin.com/in/jorgeeduardo/).