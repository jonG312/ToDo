# Usa una imagen base de Python
FROM python:3.9-slim

# Establece el directorio de trabajo en /app
WORKDIR /app

# Copia el archivo requirements.txt al directorio de trabajo
COPY requirements.txt .

# Instala las dependencias del proyecto
RUN pip install --no-cache-dir -r requirements.txt

# Copia el contenido de la carpeta de tu proyecto Django al directorio de trabajo
COPY . .

# Expone el puerto 8000 para que pueda ser accedido desde fuera del contenedor
EXPOSE 8000

# Comando para ejecutar la aplicación Django cuando se inicie el contenedor
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
