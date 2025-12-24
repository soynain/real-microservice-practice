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

Avances 23/12/2025
<img width="1237" height="347" alt="image" src="https://github.com/user-attachments/assets/e0766981-cd57-4b2d-a9af-b45cc4ea1849" />

Ya subí dos imagenes docker al Amazon ECR, sin embargo estoy teniendo dificultades al querer crear un cluster con amazon EKS
<img width="1092" height="126" alt="image" src="https://github.com/user-attachments/assets/09ec8a5b-9ea5-4b6c-9f89-f923269f0b62" />

Aun no me cuadra porque me sale este error. Seguiré investigando

Al parecer era por un tema de que se tiene que crear un access key con un usuario IAM, con privilegios de administrador
<img width="1167" height="61" alt="image" src="https://github.com/user-attachments/assets/af46cbf6-dcec-4413-be7b-9833ad59d915" />


Ok, listo, a tenet en cuenta algo importante
<img width="1552" height="213" alt="image" src="https://github.com/user-attachments/assets/24778a7d-4775-4420-b963-b09f798c7d8e" />

Repasar muy bien el concepto del IAM, y de como simplemented no debemos usar roots para manipular ciertos comandos del CLI,
con ECR no hay tanto problema pero si es buena práctica al parecer.

Solo así no tendrás errores, desde cloudformation puedes ver el progreso de la creación de tu primer cluster de EKS.

RETAKE 23/12/2025

Aquí van los comandos básicos. El aws CLI lo instalas perfecto con la documentación y configuras tus credes para el ECR:

-Desde la sección de ECR, ya te da los comandos para pushar tus imagenes
<img width="1395" height="340" alt="image" src="https://github.com/user-attachments/assets/16857167-7f7f-4e58-90f6-6153fb852769" />

Cuando ya tengas tus ECR'S pusheadas, configura EKSCTL, es el gestor CLI de Amazon para los clusters de Kubernetes. Se encarga
de configurarlo en la nube y de ayudarnos a deployar nuestros YAMLS. Después tendrás que generar tu primer yaml de cluster.

Como aquí estamos aprendiendo y nuestra especialidad no es la infraestructura, el yaml puede quedar así:

<img width="1379" height="993" alt="image" src="https://github.com/user-attachments/assets/90aa9d3b-bdc1-4179-8004-47741d1d9c7a" />

Y ejecutas el comando eksctl create cluster -f tuarchivo.yml. Te puede salir el error de:

 Error: checking AWS STS access – cannot get role ARN for current session: operation error STS: GetCallerIdentity, get identity: get credentials: failed to refresh cached credentials, no EC2 IMDS role found, operation error ec2imds: GetMetadata, canceled, context deadline exceeded

Para solucionarlo, tipea en tu CMD aws configure, te pedirá access key y secret access key.

Vete al panel de IAM, create un usuario y añadele politicas de administrador, despúés te vas a esta sección:
<img width="2092" height="959" alt="image" src="https://github.com/user-attachments/assets/d93d52b5-8be7-4051-aad7-712071403b36" />

Desde la pestaña de seguriad creas una clave de acceso y listo, con el comando

-aws configure

Metes los dos secrets, el tercer parametro vacio y en el último pones json.

Cuando vayas a crear tu primer cluster, tardará de 15 a 20 minutos, si es éxitoso, te saldrá esta pantalla:

<img width="1102" height="156" alt="image" src="https://github.com/user-attachments/assets/c3a69a21-1a4e-4090-88bc-dfe9f194ae78" />

<img width="602" height="76" alt="image" src="https://github.com/user-attachments/assets/0b863ffc-9359-4b99-abfa-eea2be1538ea" />

Listo, tu primer cluster, ahora a deployar las imagenes

Otro update más, ahora ya configuramos los ingresses:

Entonces el pex funciona así: service>ingress>ingressclass, para el service se declara un internal:
<img width="1629" height="796" alt="image" src="https://github.com/user-attachments/assets/128b4e54-d566-4bd8-984b-c721c254c13c" />

Internal service es para vincular tu app a la exposición de su puerto, el ingress para exponer paths (Istio checker) y el ingress class sirve para que el balanceador de cargas
de aws haga su chamba en base al ingress que tu estás configurando we.

Ahora en comandos, si tiene su pequeña gracia:

Cuando tienes estos 3 archivos, antes de aplicar sus respectivos kubectl apply -f blablabla... requieres de una herramiente llamada HELM para
poder instalar el AWS Load Balancer Controller, el cual te ayuda a levantar tus ingresses class a internet.

Descargalo y configura el binario en tu maquiinita como variable de entorno:

<img width="1040" height="620" alt="image" src="https://github.com/user-attachments/assets/cc42875c-af39-4d0a-9fba-e691b2759954" />

Los binarios bajalos de aqui, para su propósito básico funciona, los canarys funcionan perfecto 

<img width="2006" height="796" alt="image" src="https://github.com/user-attachments/assets/8c4caec2-b09d-420e-b90f-5a0c0a55b77e" />

Ahora, haz una instalación del AWS Load Balancer Controller con estos comandos:
*helm repo add eks https://aws.github.io/eks-charts
*helm repo update
*helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=main-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
De a guebo tienes que instalarlo en el namespace kube-system, no puedes hacerlo en el default, aún asi no hay pedo. Puedes tener namespaces separados.

Después tienes que ejecutar un comando para instalar una politica sobre el usuario de IAM que estés manipulando para poder crear tus roles

<img width="1231" height="246" alt="image" src="https://github.com/user-attachments/assets/ef4095be-aabf-44aa-935b-c61e7fe04274" />

Te saldrá este error, ejecutas eksctl utils associate-iam-oidc-provider --cluster=main-cluster --approve y ya te deja, porque debes asignar un OIDC a tu IAM, que es como
una clase de conexión entre tu consola y la de AWS.

Investiga como crear politicas, crearás una, en la sección del json copiate y pegate todo este json https://github.com/kubernetes-sigs/aws-load-balancer-controller/blob/main/docs/install/iam_policy.json
y nombralo como AWSLoadBalancerControllerIAMPolicy.

Ahora, hay que crear una politica que anexaremos con nuestro usuario IAM actual para saber tu usuario ejecuta aws sts get-caller-identity, te saldrá
un json con 3 campos. Ejecutarás el siguiente comando, sustituirás el NUMERO_DE_TU_ROL_WE con el campo account que es númerico.

eksctl create iamserviceaccount --cluster=main-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::NUMERO_DE_TU_ROL_WE:policy/AWSLoadBalancerControllerIAMPolicy --approve --override-existing-serviceaccounts

Si te sale así, es un éxito: 

<img width="1180" height="344" alt="image" src="https://github.com/user-attachments/assets/8dd24565-d4bb-428d-a1dd-239da39042e5" />

Y también lo puedes comprobar en cloudformation

<img width="1262" height="165" alt="image" src="https://github.com/user-attachments/assets/1a005e26-df34-406d-87ed-4585965c2ce0" />

Si todo te sale chido, te saldrá en el campo address de tu ingress un DNS público asignado por Amazon, y listo, tu primer despliegue

<img width="1170" height="225" alt="image" src="https://github.com/user-attachments/assets/77a19eab-83c8-497d-a9fa-fee59aa38680" />

Y puedes ver los logs de tu pod en local ^^ puesto que tienes tu imagen vinculada por medio del ingress.


<img width="1125" height="461" alt="image" src="https://github.com/user-attachments/assets/58de1942-7979-4729-81d0-f786fc7dae9c" />

<img width="1517" height="744" alt="image" src="https://github.com/user-attachments/assets/4e0c0a4a-163a-4e11-b0bc-377383265996" />


Ahora solo me falta despegar microB y configurar su bdd con aurora... Y LISTO, TU PRIMER DESPLIEGUE DE MICROS CON EKS, ECR y AURORA RDS.
