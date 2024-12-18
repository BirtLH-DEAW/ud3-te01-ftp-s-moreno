# Usar la imagen oficial de Ubuntu, en su versión última LTS
FROM ubuntu:latest

# Añadir etiquetas de información
LABEL \
    authors="Sergio Moreno" \
    version="1.0" \
    description="Servidor FTP (ProFTPD) seguro con certificado autofirmado en Ubuntu" \
    creationDate="16-12-2024"

# Actualizar Ubuntu e instalar aplicaciones necesarias
RUN apt update && apt upgrade -y && apt install -y proftpd proftpd-mod-crypto openssl

# Exponer los puertos 21, y del 50000 al 50030 para el modo pasivo de ftp
EXPOSE 21 50000-50030

# Copiar el archivo de configuración de proftpd.conf y tls.conf
COPY proftpd.conf /etc/proftpd/proftpd.conf
COPY tls.conf /etc/proftpd/tls.conf

# Descomentar el módulo mod_tls.c
RUN sed -i 's/^#\s*LoadModule mod_tls.c/LoadModule mod_tls.c/' /etc/proftpd/modules.conf

# Crear certificado autofirmado
RUN openssl req -x509 -newkey rsa:4096 \
    -keyout /etc/ssl/private/proftpd.key \ 
    -out /etc/ssl/certs/proftpd.crt \ 
    -nodes -days 365 \
    -subj "/C=ES/ST=Gipuzkoa/L=Zarautz/O=DEAW/OU=SMORENO/CN=localhost"

# Añadir usuario DEAW con password deaw. no login, home en /srv/deaw
RUN useradd deaw -d /srv/deaw -s /usr/sbin/nologin -p "$(openssl passwd -1 deaw)"
RUN mkdir /srv/deaw
COPY Recursos_Contenedor_2 /srv/deaw/Recursos_Contenedor_2
RUN chown -R deaw:deaw /srv/deaw
RUN chmod -R 755 /srv/deaw

# Inicia ProFTPD en modo no daemon
CMD ["proftpd", "--nodaemon"]