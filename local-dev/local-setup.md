# 04 · Local development

```bash
git clone https://github.com/Joshua-Igoni/CPnotejam
cd CPnotejam/django
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python notejam/manage.py test      # 21 green tests
python notejam/manage.py runserver # http://127.0.0.1:8000
```
Docker Compose
A minimal docker-compose.yml is provided (Postgres + app).
Run with:

```bash
docker compose -f docker-compose.yml up --build
```