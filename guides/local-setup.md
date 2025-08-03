# 04 · Local development

```bash
git clone https://github.com/your-org/CPnotejam
cd CPnotejam/django
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python notejam/manage.py test      # 21 green tests
python notejam/manage.py runserver # http://127.0.0.1:8000
```
Docker Compose
A minimal local-compose.yml is provided (Postgres + app).
Run with:

bash
Copy
Edit
docker compose -f local-compose.yml up --build