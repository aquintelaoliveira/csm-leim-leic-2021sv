python -m venv csm_env
csm_env/Scripts/activate

(csm_env)pip freeze > requirements.txt
(csm_env) pip install -r requirements.txt