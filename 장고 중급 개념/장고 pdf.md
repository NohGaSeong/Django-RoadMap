# Django PDF
- 웹페이지를 pdf 로 변환시켜주는 라이브러리, 기능
- 왜 쓰냐고 할 수 있곘지만 뭐 html 을 pdf 로 변환해서 메일을 보낸다던가 그럴때 사용할 수 있을 듯 싶다. 

## 함 해보자
### 라이브러리 설치
```.py
pip3 install pdfkit
```

Download 
https://wkhtmltopdf.org/downloads.html
### 코드 적용
```.py
import pdfkit

#local - wkhtmltopdf 를 다운받은 위치를 경로로
config = pdfkit.configuration(wkhtmltopdf='C:/Program Files/wkhtmltopdf/bin/wkhtmltopdf.exe')

#server - 해당 파일을 저장해 둔 위치를 경로로.
config = pdfkit.configuration(wkhtmltopdf='/usr/local/bin/wkhtmltopdf')


options = {
    'page-size': 'A4',
    'margin-top': '0.40in',
    'margin-bottom': '0.0in',
    'margin-right': '0in',
    'margin-left': '0in',
    'encoding': "UTF-8",
    'custom-header' : [
        ('Accept-Encoding', 'gzip')
    ],
    'cookie': [
        ('cookie-name1', 'cookie-value1'),
        ('cookie-name2', 'cookie-value2'),
    ],
    'no-outline': None
}


css = 'example.css'

"""
아래 셋 중 하나 원하는걸 선택, 사용

# url 주소
pdfkit.from_url('http://google.com', 'out.pdf', configuration=config)

# html 파일
pdfkit.from_file('test.html', 'out.pdf', configuration=config)

# string 문자
pdfkit.from_string('Hello!', 'out.pdf',  configuration=config)
"""

# 옵션, css는 선택.
pdfkit.from_string('Hello!', 'out.pdf',  configuration=config, options=options, css=css)
```
