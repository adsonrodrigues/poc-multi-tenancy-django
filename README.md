# POC

Problema: Configurar um serviço django com banco de dados compartilhado e esquemas separados com um banco de dados para todos os clientes, mas um esquema por cliente.

Tentativa de solução: usar a biblioteca django-tenant-schemas

```
 FAIL  python manage.py migrate_schemas
  ● As configurações e um middleware customizado foram feitas mas ao tentar aplicar o comando python manage.py migrate_schemas foi dado um erro.

    Detalhes:

root@e941a9ae3be4:/usr/local/app# python manage.py migrate_schemas
Traceback (most recent call last):
  File "manage.py", line 22, in <module>
    main()
  File "manage.py", line 18, in main
    execute_from_command_line(sys.argv)
  File "/usr/local/lib/python3.8/dist-packages/django/core/management/__init__.py", line 401, in execute_from_command_line
    utility.execute()
  File "/usr/local/lib/python3.8/dist-packages/django/core/management/__init__.py", line 395, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "/usr/local/lib/python3.8/dist-packages/django/core/management/base.py", line 322, in run_from_argv
    parser = self.create_parser(argv[0], argv[1])
  File "/usr/local/lib/python3.8/dist-packages/django/core/management/base.py", line 296, in create_parser
    self.add_arguments(parser)
  File "/usr/local/lib/python3.8/dist-packages/tenant_schemas/management/commands/migrate_schemas.py", line 20, in add_arguments
    command.add_arguments(parser)
  File "/usr/local/lib/python3.8/dist-packages/django/core/management/commands/migrate.py", line 26, in add_arguments
    parser.add_argument(
  File "/usr/lib/python3.8/argparse.py", line 1398, in add_argument
    return self._add_action(action)
  File "/usr/lib/python3.8/argparse.py", line 1761, in _add_action
    self._optionals._add_action(action)
  File "/usr/lib/python3.8/argparse.py", line 1602, in _add_action
    action = super(_ArgumentGroup, self)._add_action(action)
  File "/usr/lib/python3.8/argparse.py", line 1412, in _add_action
    self._check_conflict(action)
  File "/usr/lib/python3.8/argparse.py", line 1551, in _check_conflict
    conflict_handler(action, confl_optionals)
  File "/usr/lib/python3.8/argparse.py", line 1560, in _handle_conflict_error
    raise ArgumentError(action, message % conflict_string)
argparse.ArgumentError: argument --skip-checks: conflicting option string: --skip-checks

```

É um bug em django-tenant-schemas. Ao ler os documentos do Django, parece que eles se esqueceram de definir
requires_system_checks como False. e isso trás incompatibilidade para o django 3.1 que é o que estou usando.
Foi aberto um Pull Request https://github.com/bernardopires/django-tenant-schemas/pull/650 porém não foi aceito.
A comunidade recomendou o uso da biblioteca https://pypi.org/project/django-tenants/ que tem uma comunidade/suporte mais ativa.

