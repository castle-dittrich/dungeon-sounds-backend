# dungeon-sounds-backend
Backend for the RPG sound board

# Running Locally
- (If macOS) `brew install pyenv` [pyenv docs](https://github.com/pyenv/pyenv) (otherwise you need to install and use python 3.4.8 yourself somehow)
- (If macOS) Add `$(pyenv root)/shims:` to your `PATH` in `.bash_profile`
- (If macOS) Open a new terminal window
- `git clone https://github.com/TheEventDiscoveryCompany/dungeon-sounds-backend.git`
- `cd dungeon-sounds-backend`
- `pip install --user pipenv` [pipenv docs](https://docs.python-guide.org/en/latest/dev/virtualenvs/)
- `pipenv install`
    - if you get:
    ```
        Ignoring blinker: markers 'extra == "flask"' don't match your environment
        Looking in indexes: https://pypi.python.org/simple
        Ignoring flask: markers 'extra == "flask"' don't match your environment
        Looking in indexes: https://pypi.python.org/simple
     ```
    - try `pipenv install --ignore-pipfile`
    - If you are getting errors about mismatching subdependencies, run `pipenv lock --pre --clear` and try `pipenv install` again
- Install Postgres [postgres docs](https://www.postgresql.org/)
    - (If macOS) You can install Postgres using [Postgres.app](https://postgresapp.com/) - follow that webpage for instructions
    - Others
        - `sudo apt-get install postgresql`
        - `sudo service postgresql start`
        - `sudo passwd postgres`
        - `su postgres`
        - `psql`
            - `\password`
- `cp .env.example .env`
    - Update the `DATABASE_URL` environment variable in `.env`
        - (If macOS) `DATABASE_URL=postgresql://localhost`
        - If you used the mothod above to install Postgres, `DATABASE_URL=postgres://postgres:test@localhost` where `test` is the password
- You should be good to go!
    - To run tests: `make tests`
        - If python is not able to find dungeonsounds in the test scripts add a symlink to `./tests/`
            - `cd tests`
            - `ln -s ../dungeonsounds dungeonsounds`
    - To run tests for a specific API resource: `pipenv run python3 -W ignore -m unittest tests/resource_tests/test_user_auth.py`
        - Substitute with any resource and it will work
    - To run the server: `make local-server`

- run black in visual studio code [here](https://github.com/ambv/black#visual-studio-code)
- run black in pycharm [here](https://github.com/ambv/black#pycharm)
- run black autoformatting `make lint`

# DB Migrations

## Creating a migration
When making a change to the models file, a migration needs to be made to update the database

First create a migration script with a description:
```bash
make db-migrate
```

Then apply the migration script:
```bash
make db-upgrade
```

To downgrade the db version:
```bash
make db-downgrade
```

## Migration errors

```
alembic.util.exc.CommandError: Target database is not up to date.
```

This means that you need to apply the existing migrations to the DB by running `make db-upgrade`


# Updating python versions

When tests on Heroku fail due to an unsupported Python version, it's time to update

## Update pyenv

Install new python version:
```bash
pyenv install <version>
```

Use new version of python:
```bash
pyenv local <version>
```

## Update pipenv

Update pipenv with new version of python:
```bash
pipenv --python <version>
```

## Update Pipfile

In `Pipfile`, update the python version requirement:
```
[requires]

python_full_version = "3.6.8"
```

Generate `Pipfile.lock`:
```bash
pipenv lock
```

Install dependencies from `Pipfile.lock`:
```bash
pipenv install
```

## Test it

Run tests:
```bash
make tests
```

## Applying new python version upgrade

If you have just pulled down some changes that include a python verson upgrade, you will need to upgrade your local environment.

Install new python version:
```bash
pyenv install <version>
```

Use new version of python:
```bash
pyenv local <version>
```

You may also have to update pipenv with new version of python:
```bash
pipenv --python <version>
```

Install dependencies from `Pipfile.lock`:
```bash
pipenv install
```

