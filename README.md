# real-microservice-practice

Hola, en este repositorio conectaremos dos sencillos microservicios srping boot
con kubernetes, de tal forma que micro A pueda conectarse con micro B por medio
de REST, y con un hostname propio de kubernetes. Expondremos microA externamente y en base a eso asignaremos
un DNS para hacer una prueba lo más cercano a lo real.

Después, como me banearon cuentas en amazon, veré si puedo crearme una cuenta de AWS para hostear mi arquitectura de práctica
a la nube con EKS. Implementaremos apiproxy tal vez.... depende.

Mini reporte 17/12
Pude crear una cuenta de AWS sin temas, creo de donde me banearon fue de hbo max pero bueh.
<img width="1916" height="777" alt="image" src="https://github.com/user-attachments/assets/1abef953-e428-4b08-b3f2-bdfffd3b9dcb" />

Ya he hecho unos avances a nivel kubernetes en si, hoy aprendí a pushear imagenes generadas a docker hub, la capa gratuita
solo te ofrece imagenes gratuitas, por lo cual no es seguro. 

Sin embargo ya hemos aprendido lo básico sobre infra para poderlo profundizar más adelante en temas reales.

<img width="319" height="326" alt="image" src="https://github.com/user-attachments/assets/480a9b7f-d554-4e54-8bf4-dda3cd2dd4d6" />

Sobre micros he hecho solo dos micros sencillitos, los cuales se conectan por WebClient (en otras empresas
el estandar es usar resttemplate para evitar coaliciones o temitas con los pods), pero para la práctica
viene perfecto. No es cósdigo complejo, solo es un post de micro a a micro b, no abarcaremos prácticas más avanzadas
como implementaciones de tracing de logs por medio de #"$"#$"#"# en forma de jar :p (censurao pa no dar ideachis),
de interceptors, librerias para handlers de error con returns customizados y empaquetados en jars. No es mi intención
ofrecer un cascaron para tu trabajo papasito, es repasar conceptos de cloud de una manera sencilla.

AWS Tópicos a cubrir:

-Conexión externa con RDS (Instancia generada y probada en local)

-Orquestación de contenedores con EKS (por hacer)
-Pull de imagenes creadas con ECR para evitar pulls de docker hub (por hacer)

-Apiproxy sobre el endpoint post (sin auth porque eso significaria hacer un micro intermediario e implementar cierta herramienta
que empieza con K y termina con K) (por hacer, solo api proxy)

Avances a nivel micro

-Micros a y b creados
-Services generados

-Secrets implementados en el builder local y de docker por medio de un scroipt sh
que inyecta variables temporales por sesión de consola en un script batch aunque lo correcto
es una virtual machine ambientada ya con esas variables de acuerdo al stage de un pipeline

Solo falta el tema del despliegue. Y bueno, de ahi solo indagar el tema de porque en algunos casos
los pods pueden desconfigurarse (HPA Auto scaling).

Con eso, ya adoptaré a mi estilo el stack, arquitectura y modalidades de mi anterior experiencia laboral,
convirtiendome en un developer middle de microservicios con estandares aplicados
y capacidad de crear ecosistemas escalables desde cero.



<img width="1040" height="350" alt="image" src="https://github.com/user-attachments/assets/bf88f18a-42b6-46ba-8b0e-f93adf593679" />

Archivo secret inyectado, funcionaria igual en EKS
<img width="911" height="329" alt="image" src="https://github.com/user-attachments/assets/d98dccec-c783-40ce-9363-a10c595c1589" />

Secrets inyectados en entornos productivos
<img width="535" height="378" alt="image" src="https://github.com/user-attachments/assets/5f24b64d-e153-4f63-80b8-774b8c40b686" />

Y recuerda, si esto da flojera desarrollarlo es porque la creacion de micros es exhaustivo y siempre
y solo si, debes aplicar esta arquitectura si tienes un equipo de 15 o más developers trabajando
y con un monolito super grande.

Es la estrategia del divide y vencerás.

Mini avances 18/12 Hoy estudiamos conceptos básicos del aws, los iam, roles, grupos, security groups para instancias EC2 y conceptillos
que no me quise memorizar mucho como generaciones y tipos de instancias. También checamos los vpc's y los auto scaling group, load balancers.

Próxima tarea a realizar: configurar una IAM con acceso al servicio de amazon ECR, con privilegios de lectura y escritura,
para pushear nuestras imagenes docker al ECR. De ahí configurar el EKS.
