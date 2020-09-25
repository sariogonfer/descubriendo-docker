# El comando FROM indica la imagen para usar como imagen base.
FROM python:3

# El comando WORKDIR establece el directorio en los que se va a ejecutar los comandos ADD, RUN, ... Como un cd
WORKDIR /

# Los comandos ADD añade un fichero del sistema local al sistema de ficheros de la imagen.
ADD dc.py /
COPY dc_2.py /

# El comando RUN ejecuta el comando indicado. Otro ejemplo sería pip install requirements.txt
RUN pip install pystrich

# El comando EXPOSE informa de que la imagen escucha por cierto puerto. SOLO ES INFORMATIVO, NO PUBLICA
EXPOSE 80

# El comando ENV configura una variable de entorno.
ENV host 1.2.3.4

# El comando que se ejecuta cada vez que se arranca el contenedor.
ENTRYPOINT [ "python"]
CMD [ "./dc.py" ]
