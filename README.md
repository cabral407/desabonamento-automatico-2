# desabonamento-automatico-2
Automatizador de cancelamento de inscrições em e-mails com Zapier, GPT e Python
# Sistema de Desabonamento Automático de E-mails

Este projeto automatiza o cancelamento de inscrições em newsletters e e-mails promocionais usando Zapier, OpenAI (GPT), Webhooks e Python com Selenium.

## Componentes:
- Zapier: Gatilhos e ações automatizadas
- OpenAI: Extração de links de desinscrição
- Python + Selenium: Execução do clique no link
- Google Sheets: Registro das ações
- (Opcional) Telegram e Notion

## Execução local

### Requisitos:
- Python 3.9+
- Selenium
- ChromeDriver instalado

### Executar:
```bash
python webhook.py

4. Vá até o final da página e clique em **“Commit new file”**

---

#### 2. Crie o `webhook.py`

1. Clique novamente em **“Add file” → “Create new file”**
2. Nome do arquivo: `webhook.py`
3. Copie e cole este código Python:

[🔽 Clique para expandir o código](#)

```python
from flask import Flask, request, jsonify
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
import time

app = Flask(__name__)

def unsubscribe(link):
    options = Options()
    options.add_argument("--headless")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")

    driver = webdriver.Chrome(options=options)

    try:
        driver.get(link)
        time.sleep(3)

        buttons = driver.find_elements(By.TAG_NAME, 'a') + driver.find_elements(By.TAG_NAME, 'button')
        keywords = ['unsubscribe', 'cancelar', 'remover', 'opt-out']
        for b in buttons:
            if any(k in b.text.lower() for k in keywords):
                b.click()
                time.sleep(2)
                break

        print(f"✅ Link processado: {link}")
    except Exception as e:
        print(f"❌ Erro: {e}")
    finally:
        driver.quit()

@app.route("/unsubscribe", methods=["POST"])
def handle_unsubscribe():
    data = request.json
    link = data.get("link")
    if link:
        unsubscribe(link)
        return jsonify({"status": "Executado"}), 200
    return jsonify({"erro": "Link ausente"}), 400

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
