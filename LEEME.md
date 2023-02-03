# Playwright




* [Qué es Playwright](#Playwright)
* [Instalación](#¡Instalación!)
* [Creando nuestro primer test](#test)
* [Que queremos probar…](#probar)
* [Trace viewer](#Trace)
* [¿Cómo encontrar objetos web?](#objetos)
* [Assertions](#Assertions)
* [Grabar videos y cámara lenta](#Grabar)
* [Hooks](#Hooks)
* [Anotaciones](#Anotaciones)
* [Etiquetas](#Etiquetas)


<br>
<br>

# ¿Qué es playwright?
 1. Es un framework gratis y open source creado por microsoft para automatizar el testing web

 2. Soporta chromium, webkit y firefox

 3. Tiene una fácil preparación e instalación, además de unos test accesibles 

 4. Puede generar reportes automáticos, o a medida

 5. Compatible con Docker

 6. Puedes grabar y documentar tus test

 7. Puedes hacer test paralelos en distintos navegadores a la vez

 8. Usa shadow dom

 9. En distintos dispositivos y viewports 

 10. Test parametrizados
 

	


# ¡Instalación!
(Paso a paso y sin perdernos lo prometo)

Requisitos:
Node JS
VS Code

Instalación:
Instalación por comandos (npm)
En cualquier carpeta
npm init -y playwright@latest
Elegir Ts || Js
Nombramos ‘tests’
 Y
Nos fijamos en el verbose del comando
@playwright/test → package.json !!!
La configuración está en playwright.config.js viene auto pero podemos modificarla (no es necesario ni importante)
En la carpeta test, tenemos un ejemplo básico ya hecho ‘example.specs.js’
Se añade automáticamente al gitignore para que no tengamos que preocuparnos al subirlo a github 
.yml se usa para cosas internas de CI y github


npm playwright -v ← comprobamos si está bien instalado
Npx playwright -h ← para ver los comandos de ejecución 


	
Instalación por VS extensión
Crear una carpeta
Instalar la extension → playwright Test for VSCode
Cntrl+shit+p >‘install playwright
Seleccionamos todos y click ‘OK’
Le damos al nuevo icono y podemos ver el debugger y el play para nuestros test. Podemos ejecutar todos o alguno en concreto
Podemos elegir en que navegador ejecutar los test


	3.- Como correr test
npx playwright test (busca todos los .specs. Y los ejecuta) te generará un reporte en html (interactiva) en la carpeta playwright-report
Npx playwright show-report o abris el .html como lo haces normalmente  
npx playwright test –workers 3 
Te hará los test para 3 navegadores diferentes
npx playwright test one.specs.js (para uno en específico) (si quieres varios los pones a continuación)
  O un test individual dentro del propio test → npm playwright test -g “homepage… in title”
Npx playwright test –proyect=chromiun para una tecnología específica
Npx playwright test –headed sin UI (para alex)
Npx playwright test –debug , para debugger el test
(archivo:linea)




¡Creando nuestro primer test! :)


En la carpeta test nos creamos otro archivo de test (ex.specs.js)
Añadimos el módulo de test de playwright 
const {test, expect} = require(‘@playwriht/test’) ←esto debería de sonar ;)
Una carpeta dentro de test (ex:demo) y dentro nuestro archivo de pruebas ex.js
/test/demo/ex.js

Haremos estas dificiles funciones (podeis exportarla como querais)


Y la añadimos en el .specs.js
Const {hello, helloworld} = require(‘./demo/demo)









Creamos un bloque de tests con test()





Este error saldrá si no la hacemos asíncrona porque espera algo que tarda 

Entonces cuando ya lo tengamos arreglado usaremos expect para que espere algo de esa pagina, en nuestros proyectos usaremos local

















Veamos un ejemplo de error con un par de cosas interesantes


Como podeis ver, hemos escrito mal a posta google, aún sin saber que hay un title en la pagina q es ‘Google’, playwright hace ‘scratching’ y nos da esa salida

Podemos ver el verbose tanto en consola como en los reportes html que nos genera

Si miramos en expect.js veremos toda la gran variedad de opciones que nos proporciona


	¿Hacer test sin saber hacer test? Por supuesto
Efectivamente, aparte de los reportes, la velocidad, el soporte y las distintas operaciones en paralelo con distintos navegadores, playwright también nos permite grabar nuestros test. Una idea bastante original y cercana a la experiencia de usuario, fácil de ver y asimilar, Porque estas pruebas las haremos en directo interactuando con la página

Para ello solo tenemos que ejecutar: npx playwright codegen
 (--help para ver todos los comandos)
Por defecto nos abre en una pagina vacia y el inspector de playwright,
En esa pestaña tendremos que poner nuestra pagina (asi comprueba el acceso)
Y poniendo el ratón sobre los elemento podremos ver la magia e iremos interactuando con la pagina con el inspector abierto y veremos como genera automaticamente los test
Cuando queramos le damos al boton de grabar para que pare, podemos copiar los test,
Y en el target podemos exportarlo a cualquier lenguaje de los seleccionados
Podemos comprobarlo en la terminal 

Npx playwright codegen –target javascript -o .\test\donde lo pondremos.specs.js
Este puede dar problemas porque hace todo solo

Ej de fallo en webkit por no soportar un icono

  

Que queremos probar…

Que queremos probar en responsive? Pues…
	Npx playwright codegen –viewport-size=800,600 

Que queremos probar en un modelo especifico? Pues…
	Npx playwright codegen –device=”iPhone 11” 
(los modelos que soporta son los de las devtools de inspeccionar, si pones alguno mal o que no exista, te muestra todos los disponibles, PROBAR)


Que queremos probar el modo oscuro? Pues…
		Npx playwright codegen –color-scheme=dark (o light) 
(Npx playwright codegen –color-scheme=dark playwright.dev)

	






Usando trace viewer
El trace viewer es una herramienta GUI que nos ayuda pudiendo visualizar test a lo largo del proceso con capturas, incluye linea de tiempo y otros detalles.
(Acciones(antes, y después), Metadatos, Llamadas,Consola,Network,Recursos, Interacción de clicks…)
Básicamente podremos ver el estado de nuestra aplicación en cada punto de la misma


1.-	Abrir la configuración (playwright.config.js ) y poner     trace: ‘on-first-retry’


	
Las dos últimas líneas

Significa que recolecta el trace cuando intentamos fallar el test solo la 1era vez
Así que usaremos un test grabado y cambiaremos algo para que falle y ejecutamos el test

Si tarda mucho cambiamos el timeout




Ejecutamos y esperamos

 
Veremos algo como eso
Y abajo del todo en trace, que la traza que puedes descargar


Al hacer click nos saldrá algo como esto



Podremos ver muchos datos, explorad

También podremos acceder por comandos con npx playwright show-trace trace.zip
 


Opciones del trace viewer
‘On-first-retry’ → graba la traza solo después de fallar por primera vez
’Off’		→ ¿Quién quiere grabar?
‘On’		→ Graba por cada test (pesa mucho no recomendado)
‘Retain-on-failure’ → (Graba por cada test pero borra los exitosos)


Para settear el trace desde comandos …
	Npx playwright test –trace on 
	Npx playwright show-trace trace.zip
	
Desde navegador
	Trace.playwright.dev (arrastad el zip ahi :))

Programar auto test añadid: (no lo veremos en detalle)






¿Cómo encontrar objetos web?
 


Dar al botón de explorar para ver todos los objetos web


 
https://playwright.dev/docs/api/class-selectors
https://playwright.dev/docs/api/class-locator













Demo login test

https://practicetestautomation.com/practice-test-login/




Assertions
Comprueba o verifica
Comprueba si actual = esperado
 EJ: está o no presente, visible o invisible disponible o no, coindice el test, atributo de elemento, url…



 
Con el soft() Aunque falle el test no se detiene

 






Grabar videos y cámara lenta

En el playwright.config.js
	Antes: 


	Después:
 

Guardamos ejecutamos y abrimos el reporte, abajo del todo deberá aparecer nuestro video




Hooks
Hooks : 
	test.beforeAll → antes de todos
	test.beforeEach → antes de cada uno
	test.afterAll → después de todos
	test.afterEach → después de cada uno

Groups :
Describe → agrupa los test

test.describe(‘All my test’, ()=>{

	test(‘test1’, ()=>{})
	test(‘test2’, ()=>{})
	test(‘test3’, ()=>{})
	test(‘test4’, ()=>{})

})






Anotaciones





 
Ejemplo : 

Tags
@smoke
@reg
@sanity
@fast
@slow

Ejemplo de uso  🔽 (falta cerrar una ‘)
Para ejecutar un test con tag en específico
Npx playwriting –grep “@smoke”  (en el caso del ejemplo) 

npx playwright test --grep-invert @slow
Todos menos el @slow
