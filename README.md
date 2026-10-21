mlops-project/
├── .github/
│   └── workflows/
│       └── ci.yaml                 # uv sync + pre-commit run --all-files (copy from lessons/best-practices)
├── .pre-commit-config.yaml         # ruff format/lint/isort, nbstripout, uv-lock, whitespace/yaml/toml checks
├── .gitignore                      # ignore /data/, /models/, mlruns/, .env, .venv, *.db
├── .dockerignore                   # ignore local_data/, mlruns/, .venv, secrets, *.db
├── .python-version                 # 3.11
├── pyproject.toml                  # deps + [dependency-groups] dev + ruff config (google docstrings)
├── uv.lock                         # committed lockfile
├── Makefile                        # shortcuts: install, lint, test, train, serve, docker-build…
├── README.md                       # how to set up, train, deploy, run the API
│
├── config/                         # or a config.py inside src/
│   └── config.py                   # PROJECT_ROOT, DATA_DIRPATH, MODELS_DIRPATH, feature lists, MLflow URI
│
├── data/                           # gitignored. Raw/input data (.gitkeep only)
├── models/                         # gitignored. Serialized pipelines/preprocessors (.gitkeep only)
├── mlruns/                         # gitignored. Local MLflow tracking store
├── notebooks/                      # exploration only, outputs stripped by nbstripout
│   └── eda.ipynb
│
├── src/                            # main package (the course calls it `lib`)
│   ├── __init__.py
│   ├── modelling/                  # core ML logic, as in lessons/03 lib/
│   │   ├── __init__.py
│   │   ├── preprocessing.py        # load data, compute target, clean, encode features
│   │   ├── training.py             # train_model, evaluate_model
│   │   ├── predicting.py           # predict
│   │   └── utils.py                # load_pickle / save_pickle (helpers)
│   ├── workflows/                  # Prefect orchestration
│   │   ├── __init__.py
│   │   ├── workflows.py            # @task/@flow: train_model_workflow, batch_predict_workflow
│   │   └── deployment.py           # flow.serve(...) schedules (weekly retrain, hourly predict)
│   └── web_service/                # FastAPI app, as in lessons/02 web_service/
│       ├── __init__.py
│       ├── main.py                 # FastAPI app: home route + /predict
│       ├── app_config.py           # APP_TITLE, APP_VERSION, MODEL_VERSION, PATH_TO_PIPELINE
│       ├── lib/
│       │   ├── models.py           # pydantic input/output schemas
│       │   └── inference.py        # run_inference using the loaded model
│       └── local_models/           # model artifact copied in for the Docker image
│
├── tests/
│   ├── test_preprocessing.py
│   ├── test_training.py
│   └── test_api.py
│
├── Dockerfile.app                  # python:3.11-slim + uv sync --locked, EXPOSE 8000, uvicorn on 0.0.0.0
└── requirements_app.txt            # minimal deps for the API container (optional if you use uv in Docker)
