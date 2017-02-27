# Alwaysdata
[https://admin.alwaysdata.com/](https://admin.alwaysdata.com/)
login: Your `@antistatique` credentials

### Setup new server
If your customer isn't registered, you have to create it

### Create client

- [https://admin.alwaysdata.com/admin/account/](https://admin.alwaysdata.com/admin/account/)
- use the __Add acount__ action
- Fill in the form with your informations and the __Pack 10go__
  - go to __1password__ and duplicate the `Alwaysdata - template` entry. Then edit it and insert your project informations.
    - naming: Alwaydata - `[customer_name]` - `[project_name]`
    - Generate a strong password for the alwaysdata account
    - check that the antistatique __Vault__ is selected when saving the entry
  - select __yearly__ payment
  - select __Antistatique Paris(France)__ server

### Create vhost

- select your newly created account in the left dropdown
- __add a site__
  - use following naming convention: `[customer_name]` - `[environment]` (ex: `Cardis - Staging`)
  - if you have acces to the customer dns, you can use a domain like `env.domain.ltd`

  - otherwise you have access to antistatique following subdomains: __production__, __staging__, __test__ (ex: `cardis.staging.antistatique.net` )

  - Configuration type: `Apache standard`
  - Root directory:
    - for this, set directly the final serving folder of the website `/www/env.domain.ltd/`
      - add `/web/` for drupal
      - add `/current/` for capistrano
      - ... and take other cms/tools in account
  - force https but __do not forget__ to create a letsencrypt certificate

### Create Let's encrypt certicicate: https / SSL

- [https://admin.alwaysdata.com/ssl/](https://admin.alwaysdata.com/ssl/)
- __add a SSL certificate__
- select `create a Let's Encrypt certificate`
- add the domain you want to make SSL (created earlier) (ex: `cardis.staging.antistatique.net`)
- after saving, the certificate is listed as "incomplete". This is normal, let it like this, it will be validated in some time automatically

## Create database & database user

- https://admin.alwaysdata.com/database/?type=[your db type]
- __add database__
  - name your database as you want
  - let the rest as it is
- __User management -> Add user__
- create the user, generate passsword and insert info in __1password__
- enable `SSL connection required`
- give your new user acces only to the database he needs

## PHP
- [https://admin.alwaysdata.com/environment/php/](https://admin.alwaysdata.com/environment/php/)
- choose a php version corresponding to your needs
- save


## setup customer SSH

- [https://admin.alwaysdata.com/ssh/](https://admin.alwaysdata.com/ssh/)
- edit user
- generate password and save data in __1password__
- tick `enabled`
- save

### connect with ssh
- edit `/Users/[username]/.ssh/config`
- add a corresponding entry like:

```host cardis
   hostname antistatique.alwaysdata.net
   user cardis
   ForwardAgent yes
   IdentityFile ~/.ssh/id_rsa```

- do a ssh-copy-id [projectname] (ex: ssh-copy-id cardis)
- enter the ssh password generated earlier (stored in __1password__)
- congratulations, you are connected!
(if a prompt asks you for your password to id_rsa file, justi use the command `ssh-add -K ~/.ssh/id_rsa` )
