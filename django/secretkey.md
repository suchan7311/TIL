# Django 프로젝트 업로드시 secret key 분리

### secrets.json

```json
{
	"SECRET_KEY" : "secret key"
}
```

### settings.py

```python

import os, json
from django.core.exceptions import ImproperlyConfigured
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!

secret_file = os.path.join(BASE_DIR, 'secrets.json') #secrets.json을 불러와 줍니다.

with open(secret_file, 'r') as f: #open as로 secret.json을 열어줍니다.
    secrets = json.loads(f.read())

def get_secret(setting, secrets=secrets): #예외 처리를 통해 오류 발생을 검출합니다.
    try:
        return secrets[setting]
    except KeyError:
        error_msg = "Set the {} environment variable".format(setting)
        raise ImproperlyConfigured(error_msg)

SECRET_KEY = get_secret("SECRET_KEY")
```

