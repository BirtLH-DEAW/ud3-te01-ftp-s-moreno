# Usar la imagen oficial de Ubuntu, en su versión última LTS
FROM ubuntu:latest

# Añadir etiquetas de información
LABEL \
    authors="Sergio Moreno" \
    version="1.0" \
    description="Servidor FTP (ProFTPD) conexión anónima en Ubuntu" \
    creationDate="16-12-2024"

# Actualizar Ubuntu e instalar aplicaciones necesarias
RUN apt update && apt install -y proftpd

# Exponer los puertos 21, y del 50000 al 50030 para el modo pasivo de ftp
EXPOSE 21 50000-50030

# Copiar el archivo de configuración de proftpd.conf
COPY proftpd.conf /etc/proftpd/proftpd.conf

# Copiar los recursos a la carpeta de acceso anónimo
COPY Recursos_Contenedor_1 /srv/ftp/Recursos_Contenedor_1

# Inicia ProFTPD en modo no daemon
CMD ["proftpd", "--nodaemon"]