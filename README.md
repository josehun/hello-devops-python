# Hello DevOps Python

Ez a projekt egy egyszerű, Flask-alapú „Hello DevOps” webalkalmazás, amelyet egy mini DevOps-pipeline bemutatására készítettem.  
A cél: **kód → Git (trunk-based) → build → Docker image → CI pipeline → Docker Hub registry**.

---

1. Alkalmazás

Az alkalmazás egy egyszerű HTTP végpontot (`/`) valósít meg, amely szöveges választ ad.

- Technológia: **Python 3.12 + Flask 3**
- Belépési pont: `app/main.py`
- Alapértelmezett port: `8080`

```python
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello DevOps from Python!"

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 8080))
    app.run(host="0.0.0.0", port=port)
```
2. A projekt főbb fájljai / mappái
```
hello-devops-python/
  app/
    __init__.py
    main.py
  .github/
    workflows/
      ci.yml
  .gitignore
  requirements.txt
  Dockerfile
  README.md
```
3. Lokális futtatás (build + run)
   Előfeltétel: Python 3.10+ telepítve
   Virtuális környezet és függőségek
```   
python -m venv .venv

# Windows PowerShell:
# .\.venv\Scripts\Activate.ps1
# vagy cmd-ben:
# .\.venv\Scripts\activate.bat

pip install --upgrade pip
pip install -r requirements.txt
```
  A requirements.txt tartalma
```
flask==3.0.3
```
Alkalmazás indítása
```
python app/main.py
```
4. Git és trunk-based fejlesztés




