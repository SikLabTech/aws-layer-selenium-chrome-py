# Selenium com Chrome em AWS Lambda Python 3.7

<div>
  <img width="200px" src="https://avatars0.githubusercontent.com/u/70340698?s=200&v=4" />
  <img width="250px" src="https://i.ibb.co/nrD1FdV/imagem-2021-05-29-231908.png" />
</div>
<br />

Camada para a utilização do selenium com chrome em uma Lambda. Python 3.7 é necessário!

## Configuração

### Faça o upload do .zip em uma camada lambda 

![image](https://user-images.githubusercontent.com/41404633/120089992-7812d800-c0d5-11eb-9a18-4ffd48317017.png)

![image](https://user-images.githubusercontent.com/41404633/120090012-9a0c5a80-c0d5-11eb-9263-f23983044cd9.png)

### Adicione a camada em sua função lambda (Python 3.7)

Na seção principal da função lambda no ultimo item, adicione a camada criada.

![image](https://user-images.githubusercontent.com/41404633/120090122-74338580-c0d6-11eb-9d57-8605bad4fd0e.png)

## Exemplo da função lambda

```
from selenium.webdriver import Chrome
from selenium.webdriver.chrome.options import Options
import json, base64

def lambda_handler(event, context):
    options = Options()
    options.binary_location = '/opt/headless-chromium'
    options.add_argument('--headless')
    options.add_argument('--no-sandbox')
    options.add_argument('--start-maximized')
    options.add_argument('--start-fullscreen')
    options.add_argument('--single-process')
    options.add_argument('--disable-dev-shm-usage')

    driver = Chrome('/opt/chromedriver', options=options)

    driver.get(event.body.url)
    html = driver.page_source
    driver.quit()

    return {
        'statusCode': 200,
        'body': str(html)
    }
```
