# Hello DevOps – Python + Docker + GitHub Actions

Ez a projekt egy egyszerű, Flask-alapú „Hello DevOps” webalkalmazás, amelyen keresztül a DevOps házi feladathoz kapcsolódó követelményeket valósítottam meg:

- **2.1 – Alkalmazás (HTTP-szolgáltatás)**
- **2.2 – Buildelés (futtatható alkalmazás létrehozása)**
- **2.3 – Git használata, trunk-based development**
- **2.4 – Dockerizálás**
- **3.2 – CI pipeline + free registry (GitHub Actions + Docker Hub)**

A repository: <https://github.com/josehun/hello-devops-python>  
A Docker Hub image: `josehun/hello-devops-python:latest`

---

## 1. 2.1 – Alkalmazás (HTTP-szolgáltatás)

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

Projektstruktúra:
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

## 2. 2 – Buildelés és lokális futtatás
A cél az volt, hogy egy külső fejlesztő is pár parancsból el tudja indítani az alkalmazást.
Az alábbi lépések a README alapján reprodukálhatók.

2.2.1 Előfeltételek

Python 3.10+ telepítve

2.2.2 Virtuális környezet és függőségek
```
python -m venv .venv

# Windows PowerShell:
# .\.venv\Scripts\Activate.ps1
# Windows cmd:
# .\.venv\Scripts\activate.bat
# Linux / macOS:
# source .venv/bin/activate

pip install --upgrade pip
pip install -r requirements.txt
```
requirements.txt
flask==3.0.3

2.2.3 Alkalmazás indítása
```
python app/main.py
```
Eredmény:

http://localhost:8080
 címen az alkalmazás válasza:
„Hello DevOps from Python!”

Ezzel a 2.2 pont szerinti „build + futtatás” dokumentáltan elkészül.

## 3. 2 – Git használata, trunk-based development

A repository trunk branch-e: main.
A fejlesztés során trunk-based mintát alkalmaztam:

1. Kezdeti commit a main ágon
```
git init
git add .
git commit -m "chore: initial hello-devops python project"
git branch -M main
```
2. Feature branch létrehozása és módosítás
```
git checkout -b feature/change-message
# app/main.py-ben módosítás (pl. üzenet frissítése)
git add app/main.py
git commit -m "feat: update hello message to v2"
```
3. Merge vissza a trunkre
```
git checkout main
git merge feature/change-message
```
Ezzel teljesül a feladat követelménye:

- van egyértelmű trunk (main),
- létezik feature branch,
- azon önálló commitok vannak,
- és a branch visszakerül a trunkre (merge).
A repository GitHubon is elérhető, a commitok, branchek és a merge története látható.













