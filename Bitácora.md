# Bitácora.

## Portada:

Nombre: Joycelin Monserrat Kiau Sandoval.

Materia: Seguridad de Datos.

### Apuntes de clase 16/01/24

Bienevenida a la clase, desmotracion de diversos aparatos que se utlizan el Seguridad de Datos en donde se realizan pruebas de concepto.

### Apuntes de clase 18/01/24

Se hizo un breve introducción a los conceptos de seguridad, en donde estuvimos viendo el triangulo de CIA y se hablo sobre sus 3 puntos: 
* confidencialidad:
* Integridad:
* Disponibilidad: 

Además, tambien se dijo que debe existir un equilibrio entre los aspectos del Usability Triangle.

### Tarea 999 

<img src="./Tarea_999_crear cuenta.png" />

### Tarea 998 

<img src="./Tarea_998_crear repositorio.png" />

### Tarea 997 

<img src="./Tarea_997_Registrarse_ github_for_education.png" />

### Tarea 995: Conceptos basicos de seguridad.

| Concepto    | Definición  |
|:-------------:|:------------:|
| ""      | Confidencialidad: Garantiza que la información solo es accesible a aquellos autorizados para verla. 
| CIA Triad       |Integridad: Asegura que la información es precisa y no ha sido alterada de manera no autorizada. 
| ""     |Disponibilidad: Garantiza que la información y los recursos están disponibles cuando se necesitan.     | 
| Usability Triangle:      | Seguridad: Garantiza la protección de datos y sistemas contra accesos no autorizados.     | 
| ""    | Usabilidad: Asegura que los sistemas sean fáciles de usar y accesibles para los usuarios autorizados.      | 
| ""       | Funcionalidad: Se refiere a la capacidad del sistema para realizar sus funciones previstas.     | 
| Riesgo:       | En el contexto de la seguridad, es la probabilidad de que ocurra un evento adverso y cause un impacto negativo en ese evento.| 
| MFA (Autenticación Multifactorial):       | El MFA, o multifactor authentication, es una medida de seguridad que requiere que los usuarios proporcionen dos o más factores de autenticación para acceder a un sistema o recurso. Los factores pueden ser algo que el usuario sabe (contraseña), algo que el usuario tiene (token) y algo que el usuario es (biometría).      | 
| Vulnerabilidad:      | Es una debilidad en un sistema que puede ser explotada por una amenaza para comprometer la seguridad del sistema.     | 
| Amenaza:       | Una amenaza es una acción que puede causar un daño o perjuicio a un sistema. Las amenazas se pueden clasificar en función de su origen, como internas o externas.     | 
|Impacto:    | Es la consecuencia resultante de la explotación de una amenaza sobre un sistema. Puede incluir pérdida de datos, interrupción del servicio, daño a la reputación, entre otros.        | 

### Tarea 994 

<img src="./tarea_994_ joykiau.png" />

### Apuntes clase 23/01/24

* hack Value: aquello que pueda ser interesante o valga la pena para que alguien lo quiera.
* Target of evaluation: objetivo de un atacante.
* ExploIT:forma definida para romper la seguridad de un IT
* Zero Day Attack: es el peor enemeigo de cualquier administrado de sistemas porque no se puede hacer nada.
* Vulnerability: existencia de una debilidad, diseño o implementación.
* Dalay Chaining: Los piratas informáticos que se salen con la suya con el robo de bases de datos generalmente completan su tarea y luego retroceden para cubrir sus huellas destruyendo registros. 

sistema detector de intrusos.

* Non repudiation: para que el usuario diga que no fue el.

### Vectores de ataque 

* insideer threats 
* Mobile device security
* organized 

### Tarea 988 Transcribir los primeros 5 scripts del libro Black Hat Python for Pentesters.

<strong>CODE 1</strong>

	def sum(number_one,number_two):

		number_one_int = convert_integer(number_one)
		number_two_int = convert_integer(number_two)

		result = number_one_int + number_two_int

		return result

	def convert_integer(number_string):

		converted_integer = int(number_string)
		return converted_integer

	answer = sum("1","2")

<strong>CODE 2 TCP CLIENT</strong>

	import socket

	target_host = "www.google.com"
	target_port = 80

	#create a socket object

	client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

	#connect the client
	client.connect((target_host,target_port))

	#Send some data
	client.send("GET / HTTP/1.1\r\nHost: google.com\r\n\r\n")

	#receive some data
	response = client.recv(4096)

	print response

<strong>CODE 3 UDP CLIENT</strong>

	import socket

	target_host = "127.0.0.1"
	target_port = 80

	#create a socket object
	u client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

	#send some data
	client.sendto("AAABBBCCC",(target_host,target_port))

	#receive some data
	data, addr = client.recvfrom(4096)

	print data

<strong>CODE 4 TCP SERVER</strong>

	import socket
	import threading

	bind_ip = "0.0.0.0"
	bind_port = 9999

	server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

	server.bind((bind_ip,bind_port))

	server.listen(5)

	print "[ * ] Listening on %s:%d" % (bind_ip,bind_port)


	#this is our client-handling thread
	def handle_client(client_socket):

		# print out what the client sends
		request = client_socket.recv(1024)

		print "[ * ] Received: %s" % request

		# send back a packet
		client_socket.send("ACK!")

		client_socket.close()

	while True:
		
		client,addr = server.accept()
		
		print "[ * ] Accepted connection from: %s:%d" % (addr[0],addr[1])

		# spin up our client thread to handle incoming data
		client_handler = threading.Thread(target=handle_client,args=(client,))
		client_handler.start()

<strong>CODE 5 Replacing Netcat</strong>

	import sys
	import socket
	import getopt
	import threading
	import subprocess

	#define some global variables
	listen = False
	command = False
	upload = False
	execute = ""
	target = ""
	upload_destination = ""
	port = 0

	def usage():
		print "BHP Net Tool"
		print
		print "Usage: bhpnet.py -t target_host -p port"
		print "-l --listen - listen on [host]:[port] for ¬incoming connections"
		
		print "-e --execute=file_to_run - execute the given file upon ¬ receiving a connection"
		
		print "-c --command - initialize a command shell"
		print "-u --upload=destination - upon receiving connection upload a ¬ file and write to [destination]"
		
		print
		print
		print "Examples: "
		print "bhpnet.py -t 192.168.0.1 -p 5555 -l -c"
		print "bhpnet.py -t 192.168.0.1 -p 5555 -l -u=c:\\target.exe"
		print "bhpnet.py -t 192.168.0.1 -p 5555 -l -e=\"cat /etc/passwd\""
		print "echo 'ABCDEFGHI' | ./bhpnet.py -t 192.168.11.12 -p 135"
		sys.exit(0)

	def main():
		global listen
		global port
		global execute
		global command
		global upload_destination
		global target

		if not len(sys.argv[1:]):
			usage()

		# read the commandline options
		try:
			opts, args = getopt.getopt(sys.argv[1:],"hle:t:p:cu:", ¬ ["help","listen","execute","target","port","command","upload"])
		except getopt.GetoptError as err:
			print str(err)
			usage()

		for o,a in opts:
			if o in ("-h","--help"):
				usage()
			elif o in ("-l","--listen"):
				listen = True
			elif o in ("-e", "--execute"):
				execute = a
			elif o in ("-c", "--commandshell"):
				command = True
			elif o in ("-u", "--upload"):
				upload_destination = a

			elif o in ("-t", "--target"):
				target = a
			elif o in ("-p", "--port"):
				port = int(a)
			else:
				assert False,"Unhandled Option"

		# are we going to listen or just send data from stdin?
		if not listen and len(target) and port > 0:
		
			# read in the buffer from the commandline
			# this will block, so send CTRL-D if not sending input
			# to stdin
			buffer = sys.stdin.read()
			
			# send data off
			client_sender(buffer)
		
		# we are going to listen and potentially
		# upload things, execute commands, and drop a shell back
		# depending on our command line options above
		if listen:
			server_loop()

	main()	


### Apuntes clase 25/01/24
No se solicitaron apuntes 

### Apuntes clase 01/02/24
Se realizo una lluvia de ideas sobre los temas que hasta el momento habias visto sobre la materia, para después tener de encaragdo realizar 4 preguntas y respuestas.

°ebenwiber:

°time stack: hora exacta en la que sucede el evento.

°Elasticsearch (opensearch)- se usara para el proyecto. esta en modo cluster estructura que permite centratlizar los eventos que se generan en el sistema operativo,etc con q¿conetnga conexion de red, loes envia a una base de atos no relacional.

°Beat: agentes que instalan el dispsitvo final del cual quieres recolectar.

Elementos que se necesitan para asociar un evento a un usuario o dispositivo particular: -usuarios -tiempo -ip (puede no existir si es un elemento de windowns) -acción -recurso

correlacion con algo necesitas omologar los datos una vez centralizados.

°Logistash- normalizar los datos para poder pasarlos a elascticsearch°

°El objetivo de la estructura es para implementar un SIEM °Norma PCIDS - proteccion de datos TAREAAAA instalar docker desktop - windonws docker -linux opc2: podman

### Tarea 996 bitácora de apuntes usando markdown
<img src="./tarea_996_bitácora_apuntes_markdown.png" />

### Tarea 997 continuación Git_Education
<img src="./Tarea997_continuaciónGit_Education.png" />

### Tarea 990 instalar: docker, python, virtualBox, vagrant, wireshark
<img src="./Tarea_990 docker.png" />
<img src="./Tarea_990_python.png" />
<img src="./Tarea_990_virtualBox.png" />
<img src="./Tarea_990_wireshark.png" />