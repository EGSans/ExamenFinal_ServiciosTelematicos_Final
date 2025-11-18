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



En la ruta AWS/monitoring/prometheus encontraremos los siguientes archivos:




AWS/monitoring/prometheus/prometheus.yml   (targets correctos)
AWS/monitoring/prometheus/rules.yml        (alertas CPU>80%)



Las métricas documentadas incluidas para este proyecto son:


✔ node_cpu_seconds_total → uso real de CPU
✔ node_memory_MemAvailable_bytes → memoria disponible
✔ node_filesystem_avail_bytes → espacio libre del disco




#######################################################################################



4.	Visualización con Grafana 

4.1 Levantar grafana

cd monitoring

docker compose -f docker-compose-grafana.yml up -d


Una vez levantado puede acceder a grafana desde su navegador ingresando a http://IP_PUBLICA:3000





Una vez dentro de grafana se debe escoger la opción Add new connection y el data source debe ser prometheus 



Recuerde además indicar la ip de su instancia en la url además del puerto 9090 que es sobre el cuál se encuentra prometheus

http://IP_PUBLICA:9090





Los gráficos utilizados para este proyecto son:


✔ Gráfico de uso de CPU (%)
✔ Gráfico de uso de Memoria (%)
✔ Gauge de uso de Disco (%)
✔ Panel importado desde Grafana.com




Evidencias de despliegue:



Punto 1.	Empaquetado y despliegue local con Docker + SSL :


<img width="1908" height="975" alt="image" src="https://github.com/user-attachments/assets/2f808994-6c7b-4394-a82b-fed919e081f0" />




<img width="683" height="816" alt="image" src="https://github.com/user-attachments/assets/2de7b5bc-a133-4c99-b9ca-8955fa304421" />



Punto 2.	Despliegue en la nube con AWS EC2:

Acceso con la IP pública de la instancia + SSL 

<img width="1915" height="968" alt="image" src="https://github.com/user-attachments/assets/b1818177-04fc-444d-8248-8e6c1ec72348" />



<img width="669" height="804" alt="image" src="https://github.com/user-attachments/assets/7ddde4c2-c22b-498f-8301-4fbb8a3ab261" />



Punto 3.	Monitoreo con Prometheus y Node Exporter




<img width="1898" height="526" alt="image" src="https://github.com/user-attachments/assets/8e4ae5f1-9e6e-487f-bf5f-47e112424324" />






Punto 4.	Visualización con Grafana



<img width="749" height="749" alt="image" src="https://github.com/user-attachments/assets/33a0cdc2-2ad1-40ae-89ab-3a7fb282036a" />

<img width="841" height="375" alt="image" src="https://github.com/user-attachments/assets/fe87c0d2-0f09-41dd-8701-1bd22a369743" />






<img width="1525" height="775" alt="image" src="https://github.com/user-attachments/assets/10a09b34-d457-4e81-95df-456a00c3ab71" />







#######################################################################################









Conclusiones 




1. ¿Qué aprendí al integrar Docker, AWS y Prometheus?

Que Docker permite un entorno reproducible, AWS brinda escalabilidad real y Prometheus + Grafana agregan observabilidad profesional al proyecto. Combinados permiten un flujo DevOps moderno y sostenible.

2️. ¿Qué fue lo más desafiante?

Sin duda, integrar servicios entre sí:

Nginx reverse proxy con HTTPS

Puertos abiertos entre Node Exporter → Prometheus → Grafana

Cambios de IP pública de EC2


3️. ¿Qué beneficio aporta la observabilidad en DevOps?

Permite reaccionar antes de que el servicio caiga: alertas automáticas, métricas en tiempo real, análisis histórico y decisiones basadas en datos. Es esencial para producción.






















