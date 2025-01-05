# Heroku

Login:

```bash
heroku login
heroku container:login
```text

Create:

```bash
heroku create <app-name>
```

Build and push:

```bash
heroku container:push web --app <app-name>
```

Release:

```bash
heroku container:release web --app <app-name>
```

Add environment variable:

```bash
heroku config:set VAR_NAME=value --app <app-name>
```

Open:

```bash
heroku open --app <app-name>
```

Logs:

```bash
heroku logs --tail --app <app-name>
```

Scale:

```bash
heroku ps:scale web=1 --app <app-name>
```

Procfile:

```procfile
web: python app.py
worker: python worker.py
```
