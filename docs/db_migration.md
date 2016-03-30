Database migration
==================

In Spring 2016 the website was migrated from an old iMac in 346 Soda to the [Open Computing Facility (OCF)](http://ocf.berkeley.edu).

This documentation outlines the migration process from PostgreSQL to MySQL (but should be applicable for most migrations).

## Activate virtualenv
Run `source venv/bin/activate` before continuing to the instructions below.

## Exporting database to JSON file
`python manage.py dumpdata --natural --all --indent=4 > dump.json`

## Adding a database option
 - In upe/settings.py, add the following in DATABASES:
```
'mysql': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME': 'DB_NAME',
    'USER': 'MYSQL_USER',
    'PASSWORD': 'MYSQL_PASSWORD',
    'HOST': 'MYSQL_HOST',
    'PORT': '3306'
}
```

- Ensure that `DB_NAME` exists in the database. If using MySQL, run `mysql -u $USER -p` and enter `CREATE DATABASE DB_NAME;`).
- Run the migration script to create tables: `python manage.py migrate --database=mysql --no-initial-data`
- Ensure the database is empty (we'll import data later): `python manage.py sqlflush --database=mysql`

## Importing from a dump
`python manage.py loaddata dump.json --database=mysql`

## Switch default database
 - In upe.settings.py, change `mysql` to `default`.
 - Make sure there is only one `default` database entry.
 - Restart the web server for the changes to take effect.
