# Examen Final Servicios Telemáticos
Repositorio correspondiente al examen final de la asignatura de servicios telemáticos

#Descripción



Este repositorio contiene la solución completa al examen final de la asignatura Servicios Telemáticos, basado en el caso de estudio de la empresa ficticia CloudNova.
El objetivo es realizar el empaquetado, despliegue seguro, monitoreo y visualización de una aplicación web utilizando:

✔ Vagrant + VirtualBox


✔ Nginx con HTTPS (SSL)


✔ Docker + Docker Compose


✔ AWS EC2


✔ Prometheus + Node Exporter


✔ Grafana



#Estructura del proyecto

<img width="888" height="551" alt="image" src="https://github.com/user-attachments/assets/e8117aff-49aa-48f3-a360-04c48e99d760" />




 REQUISITOS PREVIOS (En Windows )


VirtualBox instalado

Vagrant instalado

Git instalado

Docker Desktop (opcional pero recomendado)

En AWS

Cuenta activa

Una instancia EC2 Ubuntu 22.04 o 24.04

Seguridad:

Puerto 22 (SSH)

Puerto 80 (HTTP)

Puerto 443 (HTTPS)

Puerto 3000 (Grafana)

Puerto 9090 (Prometheus)

Puerto 9100 (Node Exporter)

#######################################################################################

Punto 1 – Despliegue Local con Vagrant + Docker + HTTPS


1. Clonar el repositorio (puede realizar en la carpeta que usted desee)

git clone https://github.com/EGSans/ExamenFinal_ServiciosTelematicos.git



#######################################################################################


2. Levantar la máquina virtual




Una vez clonado el repositorio ejecutaremos el siguiente comando en la carpeta principal o general del proyecto




vagrant up



Esto crea una VM Ubuntu lista con:

✔ Docker
✔ Docker Compose
✔ Certificados SSL
✔ Redirección automática HTTP → HTTPS
✔ Puertos expuestos:

https://localhost:8443

http://localhost:8080



Tenga en cuenta que como en el archivo de configuración de nginx/default.conf  se habilitó la redirección del puerto HTTP a HTTPS aunque acceda al puerto 8080 será redirigido al puerto 8443 con nuestro sitio HTTPS con certificado autofirmado




#######################################################################################


Punto 2 – Despliegue en AWS EC2


Recuerde una vez creada su instancia puede conectarse a ella mediante ssh, por ejemplo:


ssh -i "ExamenFinal.pem" ubuntu@ec2-44-211-24-213.compute-1.amazonaws.com




1. Instalación de docker en la instancia de AWS

sudo apt update -y


sudo apt install -y docker.io docker-compose


Subir el código de MiniWebApp a nuestra instancia AWS 

scp -i ExamenFinal.pem -r MiniWebApp/ ubuntu@IP_PUBLICA:/home/ubuntu/MiniWebApp



NOTA: tenga en cuenta que el archivo .pem debe ser generado en el proceso de creación de la instancia y la IP será distinta cada vez que levante la instancia 


Una vez copiado accederemos a la carpeta de nuestra app y levantaremos el contenedor 


cd MiniWebApp


sudo docker compose up -d


Una vez levantado podremos acceder a través de la IP pública de nuestra instancia 




#######################################################################################



Punto 3 – Monitoreo con Prometheus y Node Exporter



1. Acceder a la carpeta monitoring de nuestro proyecto AWS y levantar los contenedores


cd monitoring



docker compose up -d


Esto levanta:

Prometheus → puerto 9090

Node Exporter → puerto 9100






