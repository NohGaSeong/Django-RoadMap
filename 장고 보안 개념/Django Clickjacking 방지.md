# Django Clickjacking 방지
## 클릭재킹이란?
- UI 수정 공격.
- 사용자가 본인이 인식하는 것과 다른 항목을 클릭 하도록 속여서 기밀 정보를 공개하거나 다른 사람이 자신의 컴퓨터를 제어하도록 하는 악의적인 기술임.
- 구매 버튼인데 뭐 누르면 정보같은게 흘러간다던가? 같은 느낌
## 방지?
- 브라우저에서 이에 대한 보안을 관리하고 있음.
- 자원이 Frame 또는 Iframe내에서 로드 될 수 있는지 여부를 판단하기 위해 HTTP Header의 X-Frame-Options을 사용함.
- 이 값이 SAMEORIGIN이고 같은 사이트에서 요청이 온거라고 로드하고 deny 라면 거부하게됨.

## 장고에선 우리도 모르는 사이에 해주고 있다.
### middleware
settings에 보면 XFrameOptionMiddleware 라는 미들웨어가 존재하는데 여기서 모든 response에 대해서 deny 값을 넣어주게됨.
```python
# config/settings.py

MIDDLEWARE = [
  'django.moddleware.clickjacking.XFrameOptionMiddleware',
```
deny가 아닌 다른 값을 원하면 아래와 같이 header에 설정해줄 수 있음 
`X_FRAME_OPTIONS = 'SMAEORIGIN'
