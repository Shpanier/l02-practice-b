# Stage 1: install the dependencies into a virtual environment
FROM python:3.12-slim AS build
RUN python -m venv /venv
ENV PATH="/venv/bin:$PATH"
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: a small runtime image that runs as a non-root user
FROM python:3.12-slim
WORKDIR /app
COPY --from=build /venv /venv
COPY app.py grades.py ./
ENV PATH="/venv/bin:$PATH"
USER 1000
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=3s \
  CMD python -c "import urllib.request as u; \
  u.urlopen('http://localhost:8000/health')"
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
