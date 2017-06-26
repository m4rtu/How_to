
## Criptografía asimetrica y mails cifrados con Thunderbird+enigmail+GPG

Este va a intentar ser un post para despejar las dudas sobre como cifrar mails con GPG, bueno, esta será una manera de explicarlo e implementarlo, seguro que hay miles más.

Concretamente voy a explicar como hacerlo con el cliente de correo Thunderbird y con su plugin Enigmail, que es el que permite integrar OpenPGP (un estandar de cifrado) en Thunderbird. Pero tambien necesitas un programa en tu equipo que implemente OpenPGP, éste es GPG (“Gnu Privacy Guard”). El paquete de linux es gnupg2 y para windows tenemos gpg4win (tambien hay uno para mac pero no me lo se).

Para la desambiguacion entre PGP, GPG y OpenPGP leer esto. El resumen es que:
    **PGP** es un estandar de cifrado privativo.
    **OpenPGP** es un estandar de cifrado de software libre basado en PGP.
    **GPG** es un sofware (un programa) que permite usar ambos standars (cifrar, descifrar, gestionar claves, etc). Existe una      verison grafica y otra de linea de comandos.

El resumen de lo que necesitamos instalar en nuestro equipo es:

    * Thunderbird
    * GnuPG que es lo mismo que GPG: si tienes linux tienes que instalar el paquete gnupg2 (necesitas la version 2, con la uno enigmail no funciona): en Ubuntu suele estar el gestor de software, tambien puedes hacer “apt-get install gnupg2” en la terminal;  y si tienes windows te descargas el ejecutable de gpg4win y lo instalas.
    * Añadir el plugin Enigmail (ver como instalar plugins en enigmail)

Bien, pues ahora lo que tiene que pasar es que Enigmail reconozca que has instalado GPG. En Windows, a veces, si no lo reconoce tienes que indicarle la ruta donde esta la aplicacion (En mi caso en C:\Program Files\GNU\GnuPG\gpg2). En Ubuntu, siempre vas a tener intalada la version uno de GPG por defento, viene con el sistema, entonces asegurate de intalar la version dos, que es la que te permite usar enigmail: gnupg2; Una vez instalada enigmail ya no suele dar problemas. Vamos a comprobarlo.

Vais a vuestro thunderbird, abris el asistende de Enigmail (Enigmail Wizard) y empezais a configurar, si todo va bien es que ha reconocido nuestro GPG, si no habra que indicarselo de alguna panera y asegurarse de haberse instalado el paquete adecuado.

Vale, ahora va el rollo generarse las claves y usarlas! o sea, empezar a cifrar!

Lo primero que tienes que saber es QUE vas a hacer y QUE necesitas. Tienes que saber de que va esta forma de cifrar por que si no, igual, te vas a perder… Lo que vas a hacer es hacer, entre otras cosas, es lo que se llama “cifrado asimetrico”. Esto consiste en trabajar con “pares de claves“. Los pares de claves consisten en dos claves, una publica y otra privada, que tienen relación entre ellas, se generán a la vez, una deriva de la otra y “se necesitan la una a la otra” para cifrar y descifrar. La clave pública es pública (la puedes difundir sin miedo) y la privada es privada (no tienes que enviarla a nadie, la tienes que guardar bien y solo tener acceso tu). ¿Y como trabajan? ¿como cifran? Pues con una de ellas se cifra y con la otra se descifra o a la inversa. Normalmente, para cifrar un mensaje que vas a enviar a alguien lo cifras con la clave publica de la destinataria y la destinataria lo descifrará con su clave pribada. (vuelve a leer esto otra vez).

Vale, ¿y al revés? Si cifro con mi clave privada cualquiera que tenga mi clave pública puede descifrarlo, ¿cierto? pues sí, y ¿que sentido tiene entonces? Pues en este caso servirá no para ocurtar un mensaje si no para garantizar que el mensaje solo lo has escrito tu, es decir para “firmarlo“, ya que solo tu podrías tener esa clave privada con la que lo has cifrado. (vuelve a leer esto despues de un rato).

Cosas a tener en cuenta:

-> Un mensaje cifrado con una clave pública no puede descifrarse con ella misma de nuevo. Se necesitará su clave privada asociada, la otra parte del par, son como complementarias.

-> Un mensaje cifrado con una clave privada no puede descifrarse con ella misma de nuevo. Se necesitará su clave pública asociada, la otra parte del par, son como complementarias

-> Un par de claves siempre esta asociado a una unica persona, identidad o entidad.

-> Teniendo una clave publica de alguien no se puede derivar, averiguar, su clave privada asociada (por eso es seguro enviarla o hacerla pública). Es computacionalmente muy dificil, requeriria ordenadores haciendo operaciones durante miles de años. Esto tiene que ver con el algoritmo que se usa para generar las claves: en el están presentes numeros primos y la dificultad radica en la dificultad “computacional” de factorizar numeros primos. El algoritmo que se usa en la actulidad se llama RSA con una longitud de clave de 1024 o 2028 bits. Sí, esto tiene que ver mucho con las matematicas, si te interesa saber como funciona este tipo de algoritmos tan molones puedes leer por ejemplo esto.

serveimage

Vale, ahora que ya sabemos que queremos hacer vamos a generarnos ese par de claves tan molones asociados a nuestra persona (o identidad), normalmente estan asociados un mail. Para generarlas solo hay seguir las instrucciones básicas de enigmail, sigueinte, siguiente… si hay dudas aquí se puede ver el proceso aquí en el apartado 4.2.1.

Ok, pues ahora que ya tienes tu par de claves tienes que pedir a las personas con las que te quieras comunicar su clave publica y tu les tienes que enviar la tuya. ¿Como?Pues te vas a redactar un mensaje a una de tus colegas y le das a “adjuntar clave publica” como se ve en la imagen y ya te la adjunta:VirtualBox_windows 8.1 pro_15_07_2016_14_10_37Puedes ver que la clave es solo un archivo de texto con extensión .asc (tambien hay otras extensiones). Ahora pide a tus amigas que hagan lo mismo y te la manden la suya.

Una vez teneis las claves de vuestras colegas hay que importarlas, es decir, meterlas en el gestor de claves de enigmail: Opciones>Enigmail>Administracion de claves> Archivo> Importar claves desde un fichero

VirtualBox_windows 8.1 pro_15_07_2016_14_20_37 VirtualBox_windows 8.1 pro_15_07_2016_14_21_25

Ahora ya tendrias todo lo necesario para cifrar y firmar!

Asi que ve a redactar un correo, pon el email de tu colega a quien escribes y dale a cifrar, en esta acción estarás cifrando con la clave publica de tu colega (que ya la has teneido que importar). Cuando le llegue, ella lo descifrará con su clave privada (que no tiene que importar nada porque ya la tiene, es suya). Si ella ahora te quiere contestar tendrá que tener tu clave publica importada, cifrar el mensaje con ésta y cuando te llegue tu lo descifrarás con tu clave privada (que ya la tienes).

Todo esto es un poco trabalenguas… el objetivo, si te has dado cuenta es conseguir cifrar un mensaje sin tener que enviar una “clave secreta” por un canar “inseguro” o del que puedes dudar, para poder cifrar y descifrar. Si te has fijado todas las claves que se enviar son públicas y no importa que un atacante acceda a ellas, porque necesitaria la clave privada asociada para descifrar el mensaje y no la tiene y tampoco la puede deducir.
