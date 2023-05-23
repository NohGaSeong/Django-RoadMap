# AbstracUser, AbstractBaseUser의 차이
Django의 기본 사용자 모델은 사용자 이름을 사용하여 인증 중에 사용자를 고유하게 식별합니다. 이메일 주소를 사용하려면 서브 클래스인 `AbstractUser` 또는 `AbstractBaseUser` 를 사용합니다.

## AbstractUser
- 사용자 모델의 기존 필드에 만족하고, 사용자 이름 필드만 제거하려는 경우 주로 이 옵션을 사용합니다.
- 아래는 AbstractUser를 사용한 예제입니다.
```.py
# account/models.py

from django.contrib.auth.models import AbstractUser
from django.db import models
from django.utils.translation import gettext_lazy as _

from .managers import CustomUserManager


class CustomUser(AbstractUser):
    username = None
    email = models.EmailField(_("email address"), unique=True)

    USERNAME_FIELD = "email"
    REQUIRED_FIELDS = []

    objects = CustomUserManager()

    def __str__(self):
        return self.email
```

## AbstractBaseUser
- 자신만의 완전히 완전히 새로운 사용자 모델을 생성하여 처음부터 시작하려는 경우 주로 이 옵션을 사용합니다.
- 아래는 AbstractBaseUser를 사용한 예제입니다.
```.py
# account/models.py

from django.contrib.auth.models import AbstractBaseUser, PermissionsMixin
from django.db import models
from django.utils import timezone
from django.utils.translation import gettext_lazy as _

from .managers import CustomUserManager


class CustomUser(AbstractBaseUser, PermissionsMixin):
    email = models.EmailField(_("email address"), unique=True)
    is_staff = models.BooleanField(default=False)
    is_active = models.BooleanField(default=True)
    date_joined = models.DateTimeField(default=timezone.now)

    USERNAME_FIELD = "email"
    REQUIRED_FIELDS = []

    objects = CustomUserManager()

    def __str__(self):
        return self.email

```
## 단계?
- 단계는 모두 동일합니다.
- 1. 맞춤형 사용자 모델 및 관리자 생성
- 2. settings.py 업데이트
- 3. UserCreationForm 및 UserChangeForm 양식 사용자 정의
- 4. 관리자 업데이트
