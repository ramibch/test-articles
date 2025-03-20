# How to test and deploy a Django project using Github Actions



_.github/workflows/test_and_deploy.yaml_

```yaml
name: Test and Deploy

on: [push]

jobs:
  build:
    name: build and test django app
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:12
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: testing_db
        ports:
          - 5432:5432
        # health check for making sure the service is up
        options: --health-cmd pg_isready --health-interval 10s --health-timeout 5s --health-retries 5
    steps:
      - name: Check out repo
        uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"
      - uses: actions/cache@v3
        name: Configure pip caching
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-
      - name: Install Python dependencies
        run: |
          python -m pip install -r requirements.txt
      - name: Run Tests
        env:
          USE_SQLITE3_DB: "0"
          USE_SPACES: "0"
          DJANGO_SECRET_KEY: CI_CD_TEST_KEY
          POSTGRES_DB: testing_db
          POSTGRES_HOST: localhost
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_PORT: 5432
          DEBUG: "0"
          PRODUCTION: "1"
        run: |
          python manage.py test
      - name: Deploy to server, executing remote ssh commands using ssh key
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.MY_SERVER_HOST }}
          username: ${{ secrets.MY_SERVER_USERNAME }}
          key: ${{ secrets.MY_SERVER_KEY }}
          port: ${{ secrets.MY_SERVER_PORT }}
          # fingerprint: ${{ secrets.MY_SERVER_FINGERPRINT }}
          # if a try to run the shell script I get: bash: /home/***/my_server_scripts/englishquiz/after_deployment.sh: Permission denied
          # /home/${{ secrets.MY_SERVER_USERNAME }}/my_server_scripts/englishquiz/after_deployment.sh
          script: |
            cd /home/rami/englishquiz
            git pull
            source venv/bin/activate
            python -m pip install -r requirements.txt
            python manage.py migrate
```
