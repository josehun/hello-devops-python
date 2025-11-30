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
