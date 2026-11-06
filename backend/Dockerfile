# ==========================================
# Backend image — FastAPI + model serving
# ==========================================
FROM python:3.11-slim

# System deps needed by some ML packages (xgboost/shap compile bits,
# and general build tooling) — kept minimal, removed after install
# to keep the final image smaller.
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Install Python deps first, separately from the app code, so Docker
# can cache this layer and skip reinstalling everything just because
# a .py file changed.
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# App code + the trained model artifacts (readmission_model.joblib,
# feature_columns.json, category_options.json) all need to be present
# here — copy everything else in requirements/.dockerignore excludes
# what shouldn't ship (see .dockerignore).
COPY . .

EXPOSE 8000

# No --reload here — that's a development convenience, not something
# you want in a running container.
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
