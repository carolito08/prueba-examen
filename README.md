# prueba-examen
### 1. Creamos una imagen con un Dockerfile :
docker build . -t nombre-nueva-imagen

### 2. Levantamos 3 dockers (teniendo ya la imagen nueva creada):   	
				* docker run -d -p 8082:8080 -e PBA=calcu1 nombre-nueva-imagen
				* docker run -d -p 8083:8080 -e PBA=calcu2 nombre-nueva-imagen
				* docker run -d -p 8084:8080 -e PBA=calcu3 nombre-nueva-imagen
																									
### 3. Balanceamos con haproxy de la siguiente manera:
*  #### Actualizamos ubuntu : 
		- sudo apt-get update
*  #### Instalamos haproxy :
		- sudo apt-get install haproxy
*  #### Modificamos el archivo sudo nano /etc/default/haproxy :
		- descomentamos la linea CONFIG="/etc/haproxy/haproxy.cfg"
		- y al final del archivo agregamos la siguiente línea : ENABLED=1
*  #### Editamos el archivo de configuración :
		- sudo nano /etc/haproxy/haproxy.cfg
*  #### Finalmente agregamos las siguientes lìneas :
		- frontend www 
			- bind 0.0.0.0:9090 
			- default_backend site_backend

		- backend site_backend 
			- mode http stats enable 
			- stats uri /haproxy?stats balance roundrobin  
			- server lamp1 localhost:2020 check 
			- server lamp2 localhost:3030 check  
			- server lamp3 localhost:4040 check
	*  #### Detenemos e iniciamos el servicio haproxy:
		- sudo service haproxy stop
		- sudo service haproxy start
		
	*  #### Tecleamos la siguiente liga:
		- http://localhost:9090/api/calcu?tipo=divide&num1=1225&num2=44
		
			
