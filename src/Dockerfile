FROM python:3.11-slim
# Definir diretório de trabalho
WORKDIR /app
# Copiar requirements e instalar dependências Python
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
# Copiar código da aplicação
COPY . .
# Criar usuário não-root
RUN adduser --disabled-password --gecos '' appuser && \
    chown -R appuser:appuser /app
USER appuser
# Criar diretório para métricas Prometheus
RUN mkdir -p /tmp/prometheus_multiproc
# Expor porta
EXPOSE 8000
# Comando para iniciar a aplicação com Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "4", "main:app"]