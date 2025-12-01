# Hello DevOps – Python + Docker + GitHub Actions

Ez a projekt egy egyszerű, Flask-alapú „Hello DevOps” webalkalmazás, amelyen keresztül a DevOps házi feladathoz kapcsolódó követelményeket valósítottam meg:

- **1 – Alkalmazás (HTTP-szolgáltatás)**
- **2 – Buildelés (futtatható alkalmazás létrehozása)**
- **3 – Git használata, trunk-based development**
- **4 – Dockerizálás**
- **5 – CI pipeline + free registry (GitHub Actions + Docker Hub)**
- **6 – Összefoglalás**

A repository: <https://github.com/josehun/hello-devops-python>  
A Docker Hub image: `josehun/hello-devops-python:latest`

---

## 1 – Alkalmazás (HTTP-szolgáltatás)

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

## 2 – Buildelés és lokális futtatás
A cél az volt, hogy egy külső fejlesztő is pár parancsból el tudja indítani az alkalmazást.
Az alábbi lépések a README alapján reprodukálhatók.

2.1 Előfeltételek

Python 3.10+ telepítve

2.2 Virtuális környezet és függőségek
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

2.3 Alkalmazás indítása
```
python app/main.py
```
Eredmény:

http://localhost:8080
 címen az alkalmazás válasza:
„Hello DevOps from Python!”

## 3 – Git használata, trunk-based development

A repository trunk branch-e: main.
A fejlesztés során trunk-based mintát alkalmaztam:

3.1. Kezdeti commit a main ágon
```
git init
git add .
git commit -m "chore: initial hello-devops python project"
git branch -M main
```
3.2. Feature branch létrehozása és módosítás
```
git checkout -b feature/change-message
# app/main.py-ben módosítás (pl. üzenet frissítése)
git add app/main.py
git commit -m "feat: update hello message to v2"
```
3.3. Merge vissza a trunkre
```
git checkout main
git merge feature/change-message
```

## 4 – Dockerizálás
A projekt gyökerében található Dockerfile segítségével az alkalmazás konténerben is futtatható.
```
FROM python:3.12-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app

ENV PORT=8080
EXPOSE 8080

CMD ["python", "app/main.py"]
```
4.1 Docker image buildelése lokálisan
Ha a gépen elérhető a Docker (pl. Docker Desktop vagy Docker Engine):
```
docker build -t hello-devops-python:v1 .
docker run -p 8080:8080 hello-devops-python:v1
```

## 5 – CI pipeline + free registry (GitHub Actions + Docker Hub)

CI pipeline, amely buildeli az alkalmazást, Docker image-et készít, és egy ingyenes registry-be (Docker Hub) pusholja.

5.1 CI workflow – .github/workflows/ci.yml

A GitHub Actions workflow:

trigger: minden main branchre történő push és pull_request,

lépések:

- I. kód checkout (actions/checkout),
- II. Python 3.12 környezet beállítása,
- III. függőségek telepítése (pip install -r requirements.txt),
- IV. opcionális kódfordítás (python -m compileall app),
- V. Docker Hub login GitHub Secrets segítségével,
- VI. Docker image build + push a Docker Hubra.
```
- name: Log in to Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKERHUB_USERNAME }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}

- name: Build and push Docker image
  uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: josehun/hello-devops-python:latest
```
5.2 Docker Hub repository

A CI pipeline a következő Docker Hub repo-ba pushol:

- Repository: josehun/hello-devops-python
- Tag: latest
- URL: https://hub.docker.com/r/josehun/hello-devops-python

Sikeres futás után az image bármely gépről lehúzható és futtatható:
```
docker pull josehun/hello-devops-python:latest
docker run -p 8080:8080 josehun/hello-devops-python:latest
```

5.3 Image használata más gépen (pl. Ubuntu)

A CI által előállított image-et egy külön Ubuntu gépen (VM-en) is kipróbáltam:

Image lehúzása Docker Hubról
```
sudo docker pull josehun/hello-devops-python:latest
```
Konténer indítása
```
sudo docker run -d -p 8080:8080 --name hello-devops josehun/hello-devops-python:latest
```
Teszt
```
curl http://localhost:8080
# -> "Hello DevOps from Python!"
```


## 6 Összefoglalás

A projektben:

- 1 – Elkészült egy egyszerű Python/Flask alapú HTTP-szolgáltatás.
- 2 – Dokumentáltam a buildelés és a lokális futtatás lépéseit (venv, pip, futtatás).
- 3 – Git + trunk-based fejlesztési modell valósult meg (main + feature branch + merge).
- 4 – Az alkalmazás Docker konténerben is futtatható egy Dockerfile segítségével.
- 5 – GitHub Actions CI pipeline buildeli és a Docker Hubra pusholja az image-et. + Ubuntu teszt




