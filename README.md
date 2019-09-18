

# angular-es-code-styles
Transcripción de reglas de estilo angular
Fuente: https://angular.io/guide/styleguide

# Vocabulario de estilo
Cada guía describe una buena o mala práctica, y todas tienen una presentación consistente.

La redacción de cada directriz indica cuán fuerte es la recomendación.

**Do** es uno que siempre debe seguirse. Siempre puede ser una palabra demasiado fuerte. Las pautas que literalmente siempre deben seguirse son extremadamente raras. Por otro lado, necesita un caso realmente inusual para romper una directriz Do.

**Consider** las pautas que generalmente se deben seguir. Si comprende completamente el significado detrás de la guía y tiene una buena razón para desviarse, hágalo. Por favor, esforzarse por ser coherente.

**Avoid** indica algo que casi nunca debes hacer. Los ejemplos de código para evitar tienen un encabezado rojo inconfundible.

**¿Por qué?** da razones para seguir las recomendaciones anteriores.

**Responsabilidad única**
Aplique el principio de responsabilidad única (SRP) a todos los componentes, servicios y otros símbolos. Esto ayuda a que la aplicación sea más limpia, más fácil de leer y mantener, y más comprobable.

# Regla de uno
## Estilo 01-01
Defina una cosa, como un servicio o componente, por archivo.
Considere limitar los archivos a 400 líneas de código.

**¿Por qué?** Un componente por archivo hace que sea mucho más fácil leer, mantener y evitar colisiones con equipos en el control de código fuente.

**¿Por qué?** Un componente por archivo evita errores ocultos que a menudo surgen al combinar componentes en un archivo donde pueden compartir variables, crear cierres no deseados o un acoplamiento no deseado con dependencias.

**¿Por qué?** Un solo componente puede ser la exportación predeterminada para su archivo, lo que facilita la carga diferida con el enrutador.

La clave es hacer que el código sea más reutilizable, más fácil de leer y menos propenso a errores.

El siguiente ejemplo negativo define AppComponent, inicia la aplicación, define el objeto modelo Hero y carga héroes del servidor en el mismo archivo. 
> No hagas esto.

app / heroes / hero.component.ts

```javascript
importar {Component, NgModule, OnInit} desde '@ angular / core';
importar {BrowserModule} desde '@ angular / platform-browser';
importar {platformBrowserDynamic} desde '@ angular / platform-browser-dynamic';

héroe de clase {
  número de identificación;
  nombre: cadena;
}

@Componente({
  selector: 'app-root',
  plantilla: `
      <h1> {{title}} </h1>
      <pre> {{héroes | json}} </pre>
    `,
  styleUrls: ['app / app.component.css']
})
La clase AppComponent implementa OnInit {
  title = 'Tour de los héroes';

  héroes: Héroe [] = [];

  ngOnInit () {
    getHeroes (). then (heroes => (this.heroes = heroes));
  }
}

@NgModule ({
  importaciones: [BrowserModule],
  declaraciones: [AppComponent],
  exportaciones: [AppComponent],
  bootstrap: [componente de la aplicación]
})
clase de exportación AppModule {}

platformBrowserDynamic (). bootstrapModule (AppModule);

const HEROES: Hero [] = [
  {id: 1, nombre: 'Bombasto'},
  {id: 2, nombre: 'Tornado'},
  {id: 3, nombre: 'Magneta'}
];

función getHeroes (): Promesa <Héroe []> {
  return Promise.resolve (HÉROES); // TODO: obtener datos del héroe del servidor;
}
```

Es una mejor práctica redistribuir el componente y sus clases de soporte en sus propios archivos dedicados.

main.ts
```
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```
app / app.module.ts
```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouterModule } from '@angular/router';

import { AppComponent } from './app.component';
import { HeroesComponent } from './heroes/heroes.component';

@NgModule({
  imports: [
    BrowserModule,
  ],
  declarations: [
    AppComponent,
    HeroesComponent
  ],
  exports: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

app / app.component.ts
```
import { Component } from '@angular/core';

import { HeroService } from './heroes';

@Component({
  selector: 'toh-app',
  template: `
      <toh-heroes></toh-heroes>
    `,
  styleUrls: ['./app.component.css'],
  providers: [HeroService]
})
export class AppComponent {}
```
app / heroes / heroes.component.ts
```
import { Component, OnInit } from '@angular/core';

import { Hero, HeroService } from './shared';

@Component({
  selector: 'toh-heroes',
  template: `
      <pre>{{heroes | json}}</pre>
    `
})
export class HeroesComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) {}

  ngOnInit() {
    this.heroService.getHeroes()
      .then(heroes => this.heroes = heroes);
  }
}
```

app / heroes / shared / hero.service.ts
```
import { Injectable } from '@angular/core';

import { HEROES } from './mock-heroes';

@Injectable()
export class HeroService {
  getHeroes() {
    return Promise.resolve(HEROES);
  }
}
```

app / heroes / shared / hero.model.ts
```
export class Hero {
  id: number;
  name: string;
}
```

app / heroes / shared / mock-heroes.ts
```
import { Hero } from './hero.model';

export const HEROES: Hero[] = [
  { id: 1, name: 'Bombasto' },
  { id: 2, name: 'Tornado' },
  { id: 3, name: 'Magneta' }
];
```

A medida que la aplicación crece, esta regla se vuelve aún más importante.

# Funciones pequeñas
## Estilo 01-02
Definir funciones pequeñas

Considere limitar a no más de 75 líneas.

**¿Por qué?** Las funciones pequeñas son más fáciles de probar, especialmente cuando hacen una cosa y tienen un propósito.

**¿Por qué?** Pequeñas funciones promueven la reutilización.

**¿Por qué?** Las funciones pequeñas son más fáciles de leer.

**¿Por qué?** Las funciones pequeñas son más fáciles de mantener.

**¿Por qué?** Las funciones pequeñas ayudan a evitar errores ocultos que vienen con funciones grandes que comparten variables con un alcance externo, crean cierres no deseados o acoplamiento no deseado con dependencias.


# Nombrar
Las convenciones de nomenclatura son muy importantes para la mantenibilidad y la legibilidad. Esta guía recomienda convenciones de nomenclatura para el nombre del archivo y el nombre del símbolo.

# Pautas generales de nomenclatura
## Estilo 02-01

**Do** Utilice nombres consistentes para todos los elementos.
**Do** Siga un patrón que describa la característica del elemento y luego su tipo. El patrón recomendado es feature.type.ts.

**¿Por qué?** Las convenciones de nomenclatura ayudan a proporcionar una forma coherente de buscar contenido de un vistazo. 

**¿Por qué?** Las convenciones de nomenclatura ayudan a proporcionar una forma coherente de buscar contenido de un vistazo. La consistencia dentro del proyecto es vital. La consistencia con un equipo es importante. La consistencia en una empresa proporciona una eficiencia tremenda.

**¿Por qué?** Las convenciones de nomenclatura simplemente deberían ayudar a encontrar el código deseado más rápido y facilitar su comprensión.

**¿Por qué?** Los nombres de carpetas y archivos deben transmitir claramente su intención. Por ejemplo, app / heroes / hero-list.component.ts puede contener un componente que gestiona una lista de héroes.

# Separe los nombres de archivo con puntos y guiones
## Estilo 02-02

**Do** Utilice guiones para separar palabras en el nombre descriptivo.

**Do** Utilice puntos para separar el nombre descriptivo del tipo.

**Do** Utilice nombres de tipo coherentes para todos los componentes siguiendo un patrón que describa la característica del componente y luego su tipo. Un patrón recomendado es feature.type.ts.

**Do** Utilice nombres de tipo convencionales, incluidos .service, .component, .pipe, .module y .directive. Si es necesario, invente nombres de tipos adicionales, pero tenga cuidado de no crear demasiados.

**¿Por qué?** Los nombres de tipo proporcionan una forma coherente de identificar rápidamente lo que hay en el archivo.

**¿Por qué?** Los nombres de tipo facilitan la búsqueda de un tipo de archivo específico utilizando un editor o las técnicas de búsqueda difusa de IDE.

**¿Por qué?** Los nombres de tipo no abreviados como .service son descriptivos y no ambiguos. Las abreviaturas como .srv, .svc y .serv pueden ser confusas.

**¿Por qué?** Los nombres de tipo proporcionan coincidencia de patrones para cualquier tarea automatizada.

# Elementos y nombres de archivos
## Estilo 02-03

**Do** Utilice nombres consistentes para todos los activos nombrados después de lo que representan.

**Do** Utilice upper camel case para los nombres de clase.

**Do** Haga coincidir el nombre del Elemento con el nombre del archivo.

**Do** Agregue el nombre del elemento con el sufijo convencional (como Componente, Directiva, Módulo, Tubería o Servicio) para algo de ese tipo.

**Do** Proporcione al nombre de archivo el sufijo convencional (como .component.ts, .directive.ts, .module.ts, .pipe.ts o .service.ts) para un archivo de ese tipo.

**¿Por qué?** Las convenciones consistentes facilitan la identificación y referencia rápida de activos de diferentes tipos.


| Nombre de Elemento     | Nombre del archivo |
| ------------- |:-------------:|
| ```export class AppComponent {}``` | ```app.component.ts``` |
|```export class HeroesComponent {}``` | ``` heroes.component.ts ```   |
| ``` export class HeroListComponent {} ``` | ``` hero-list.component.ts ``` |
|``` export class HeroDetailComponent {} ``` | ``` hero-detail.component.ts ```|
| ``` export class ValidationDirective {} ```| ``` validation.directive.ts ``` |
| ``` export class AppModule ```| ``` app.module.ts ```|
|``` export class InitCapsPipe implementa PipeTransform {} ```| ``` init-caps.pipe.ts ``` |
|``` export class UserProfileService {} ``` | ``` user-profile.service.ts ``` |

# Nombres de servicio
## Estilo 02-04
**Do** Utilice nombres consistentes para todos los servicios nombrados después de su función.

**Do** Sufije un nombre de clase de servicio con Servicio. Por ejemplo, algo que obtiene datos o héroes debería llamarse DataService o HeroService.

Algunos términos son servicios inequívocos. Normalmente indican agencia terminando en "-er". Es posible que prefiera nombrar un servicio que registre los mensajes Logger en lugar de LoggerService. Decide si esta excepción es aceptable en tu proyecto. Como siempre, luche por la consistencia.

**¿Por qué?** Proporciona una manera consistente de identificar y referenciar servicios rápidamente.

**¿Por qué?** Los nombres de servicio claros como Logger no requieren un sufijo.

**¿Por qué?** Los nombres de servicios como Crédito son sustantivos y requieren un sufijo, y deben nombrarse con un sufijo cuando no es obvio si se trata de un servicio u otra cosa.


| Nombre de Elemento     | Nombre del archivo |
| ------------- |:-------------:|
|``` export class HeroDataService {} ```| ```hero-data.service.ts``` |
| ```export class  CreditService {} ``` | ``` credit.service.ts ```   |
| ```export class Logger { }``` | ``` logger.service.ts ``` |


# Bootstrapping
## Estilo 02-05

**Do** Ponga bootstrapping y lógica de plataforma para la aplicación en un archivo llamado main.ts.

**Do** Incluya el manejo de errores en la lógica de arranque.

**Do** Evite poner la lógica de la aplicación en main.ts. En cambio, considere colocarlo en un componente o servicio.

**¿Por qué?** Sigue una convención consistente para la lógica de inicio de una aplicación.

**¿Por qué?** Sigue una convención familiar de otras plataformas tecnológicas.

main.ts
```
importar {platformBrowserDynamic} desde '@ angular / platform-browser-dynamic';

importar {AppModule} desde './app/app.module';

platformBrowserDynamic (). bootstrapModule (AppModule)
  .then (success => console.log (`Bootstrap success`))
  .catch (err => console.error (err));
```

# Selectores de componentes
## Estilo 05-02

**Do** Use dashed-case or kebab-case para nombrar los selectores de elementos de los componentes.

**¿Por qué?** Mantiene los nombres de los elementos consistentes con la especificación de los Elementos personalizados.

app / heroes / shared / hero-button / hero-button.component.ts

> *evitar*
```
@Componente({
  selector: 'tohHeroButton',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent {}
```
> Ejemplo Positivo

app / heroes / shared / hero-button / hero-button.component.ts

```
@Component({
  selector: 'toh-hero-button',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent {}

```

app / app.component.html
```
<toh-hero-button></toh-hero-button>
```


# Prefijo personalizado de componente
## Estilo 02-07

**Do** Utilice un valor de selector de elemento en minúsculas con guión; por ejemplo, *admin-users* .

**Do** Utilice un prefijo personalizado para un selector de componentes. Por ejemplo, el prefijo toh representa Tour of Heroes y el prefijo admin representa un área de funciones de administrador.

**Do** Utilice un prefijo que identifique el área de características o la aplicación en sí.

**¿Por qué?** Evita las colisiones de nombres de elementos con componentes en otras aplicaciones y con elementos HTML nativos.

**¿Por qué?** Facilita la promoción y el uso compartido del componente en otras aplicaciones.

**¿Por qué?** Los componentes son fáciles de identificar en el DOM.

app / heroes / hero.component.ts

> *evitar*
```
// HeroComponent está en la función Tour of Heroes
@Componente({
  selector: 'héroe'
})
export class HeroComponent {}
```
app / users / users.component.ts

> *evitar*
```
// UsersComponent está en una función de administrador
@Componente({
  selector: 'usuarios'
})
clase de exportación UsersComponent {}
```
app / heroes / hero.component.ts
```
@Componente({
  selector: 'toh-hero'
})
export class HeroComponent {}
```
app / users / users.component.ts
```
@Componente({
  selector: 'admin-users'
})
clase de exportación UsersComponent {}
```
# Selectores de directivas
## Estilo 02-06

**Do** Utilice minúsculas de camello para nombrar los selectores de directivas.

**¿Por qué?** Mantiene los nombres de las propiedades definidas en las directivas vinculadas a la vista coherentes con los nombres de los atributos.

**¿Por qué?** El analizador HTML angular distingue entre mayúsculas y minúsculas y reconoce las minúsculas de camello.

# Prefijo personalizado de directiva
## Estilo 02-08

**Do** Utilice un prefijo personalizado para el selector de directivas (por ejemplo, el prefijo toh de Tour of Heroes).

**Do** Deletree selectores que no sean elementos en  lower camel case a menos que el selector esté destinado a coincidir con un atributo HTML nativo.

**¿Por qué?** Previene colisiones de nombres.

**¿Por qué?** Las directivas se identifican fácilmente.

app / shared / validate.directive.ts
> *evitar*

```
@Directiva({
  selector: '[validar]'
})
clase de exportación ValidateDirective {}
```
app / shared / validate.directive.ts
```
@Directiva({
  selector: '[tohValidate]'
})
clase de exportación ValidateDirective {}
```
# Nombres de los pipe
## Estilo 02-09

**Do** Utilice nombres consistentes para todas los pipe, nombrados por su característica. El nombre de la clase pipe debe usar UpperCamelCase (la convención general para los nombres de clase), y la cadena de nombre correspondiente debe usar lowerCamelCase. La cadena de nombre no puede usar guiones ("dash-case" or "kebab-case").

**¿Por qué?** Proporciona una forma consistente de identificar y hacer referencia rápidamente a las tuberías.

| Nombre de Elemento     | Nombre del archivo |
| ------------- |:-------------:|
|``` export class EllipsisPipe implementa PipeTransform {} ```| ```ellipsis.pipe.ts``` |
| ``` export class InitCapsPipe implementa PipeTransform {} ``` | ``` init-caps.pipe.ts ```   |

# Nombres de archivo de prueba unitaria
## Estilo 02-10

**Do** Nombre los archivos de especificación de prueba igual que el componente que prueban.

**Do** Nombre los archivos de especificación de prueba con un sufijo de .spec.

**¿Por qué?** Proporciona una forma consistente de identificar rápidamente las pruebas.

**¿Por qué?** Proporciona coincidencia de patrones para karma u otros corredores de prueba.

| Tipo de prueba | Nombre del archivo |
| ------------- |:-------------:|
| Componentes | heroes.component.spec.ts|
||hero-list.component.spec.ts|
||hero-deta|il.component.spec.ts |
|Servicios|| logger.service.spec.ts|
||hero.service.spec.ts|
||filter-text.service.spec.ts  |
|Pipes| ellipsis.pipe.spec.ts|
||init-caps.pipe.spec.ts |


# Nombres de archivo de prueba de extremo a extremo (E2E)
## Estilo 02-11

Nombre los archivos de especificación de prueba de extremo a extremo después de la característica que prueban con un sufijo .e2e-spec.

¿Por qué? Proporciona una forma consistente de identificar rápidamente las pruebas de extremo a extremo.

¿Por qué? Proporciona coincidencia de patrones para corredores de prueba y automatización de compilación.

|Tipo de prueba | Nombre del archivo |
|:---:|:----:|
|End-to-End Tests|app.e2e-spec.ts|
||heroes.e2e-spec.ts|


# Nombramiento de modulos - NgModule
## Estilo 02-12

**Do** Agregue el nombre del elemento con el sufijo Módulo.

**Do** Asigne al nombre del archivo la extensión .module.ts.

**Do** Nombre el módulo después de la función y la carpeta en la que reside.

**¿Por qué?** Proporciona una forma coherente de identificar y hacer referencia rápidamente a los módulos.

**¿Por qué?** Upper camel case es convencional para identificar objetos que pueden ser instanciados usando un constructor.

**¿Por qué?** Identifica fácilmente el módulo como la raíz de la característica con el mismo nombre.

**Do** Sufije un nombre de clase RoutingModule con RoutingModule.

**Do** Finalice el nombre de archivo de un RoutingModule con -routing.module.ts.

**¿Por qué?** Un RoutingModule es un módulo dedicado exclusivamente a la configuración del enrutador angular. Una convención de clase y nombre de archivo coherente hace que estos módulos sean fáciles de detectar y verificar.


<table>
<tr>
<th>
Nombre del elemento
</th>
<th>
Nombre del archivo
</th>
</tr>

<tr>
<td>
<pre>
@NgModule ({...})
export class AppModule {}
</pre>
</td>

<td>
<pre>
app.module.ts
</pre>
</td>
</tr>

<tr>
<td>
<pre>
@NgModule ({...})
export class HeroesModule {}
</pre>
</td>

<td>
<pre>
heroes.module.ts
</pre>
</td>
</tr>

<tr>
<td>
<pre>
@NgModule ({...})
export class VillainsModule {}
</pre>
</td>

<td>
<pre>
villains.module.ts
</pre>
</td>
</tr>

<tr>
<td>
<pre>
@NgModule ({...})
clase de exportación AppRoutingModule {}
</pre>
</td>

<td>
<pre>
app-routing.module.ts
</pre>
</td>
</tr>

<tr>
<td>
<pre>
@NgModule ({...})
export class HeroesRoutingModule {}
</pre>
</td>

<td>
<pre>
heroes-routing.module.ts
</pre>
</td>
</tr>
</table>



# Estructura de aplicación y NgModules

*Tener una visión a largo plazo de la implementación y una visión a largo plazo.* Comience con poco, pero tenga en cuenta hacia dónde se dirige la aplicación.

Todo el código de la aplicación va en una carpeta llamada src. Todas las áreas de características están en su propia carpeta, con su propio NgModule.

Todo el contenido es un elemento por archivo. Cada componente, servicio y tubería está en su propio archivo. Todos los scripts de proveedores externos se almacenan en otra carpeta y no en la carpeta src. No los escribiste y no los quieres abarrotados de src. Use las convenciones de nomenclatura para archivos en esta guía.

## Lift
### Estilo 04-01

**Do** Estructure la aplicación de modo que pueda localizar el código rápidamente, identifique el código de un vistazo, mantenga la estructura más plana que pueda.

**Do** Defina la estructura para seguir estas cuatro pautas básicas, enumeradas en orden de importancia.

**¿Por qué?** LIFT proporciona una estructura consistente que escala bien, es modular y hace que sea más fácil aumentar la eficiencia del desarrollador al encontrar código rápidamente. Para confirmar su intuición sobre una estructura particular, preguntese: *¿puedo abrir rápidamente y comenzar a trabajar en todos los archivos relacionados para esta función?*


## Locate
### Estilo 04-02
**Do** Localice el codigo de forma intuitiva, simple y rápida.

**¿Por qué?** Para trabajar de manera eficiente, debe poder encontrar archivos rápidamente, especialmente cuando no conoce (o no recuerda) los nombres de los archivos. Mantener los archivos relacionados cerca uno del otro en una ubicación intuitiva ahorra tiempo. Una estructura de carpetas descriptiva hace una gran diferencia para usted y las personas que vienen después de usted.


## Identify
### Estilo 04-03

**Do** Asigne un nombre al archivo de manera que sepa instantáneamente lo que contiene y representa.

**Do** Sea descriptivo con los nombres de archivo y mantenga el contenido del archivo exactamente en un componente.

**Do** Evite archivos con múltiples componentes, múltiples servicios o una mezcla.

**¿Por qué?** Pase menos tiempo buscando y picoteando códigos, y sera más eficiente. Los nombres de archivo más largos son mucho mejores que los nombres abreviados cortos pero confusos.

> Puede ser ventajoso desviarse de la regla de una cosa por archivo cuando tiene un conjunto de características pequeñas y estrechamente relacionadas que se descubren y entienden mejor en un solo archivo que como múltiples archivos. Ten cuidado con esta practica.

## Flat
### Estilo 04-04
**Do** Mantenga una estructura de carpeta plana el mayor tiempo posible.

**Do** Considere crear subcarpetas cuando una carpeta llegue a siete o más archivos.

**Considere** configurar el IDE para ocultar archivos irrelevantes y distractores, como los archivos .js y .js.map generados.

**¿Por qué?** Nadie quiere buscar un archivo a través de siete niveles de carpetas. Una estructura plana es fácil de escanear.

Por otro lado, los psicólogos creen que los humanos comienzan a luchar cuando el número de cosas interesantes adyacentes supera las nueve. Entonces, cuando una carpeta tiene diez o más archivos, puede ser el momento de crear subcarpetas.

Base su decisión en su nivel de comodidad. Use una estructura más plana hasta que haya un valor obvio para crear una nueva carpeta.

## T-DRY (Try to be DRY)
### Estilo 04-05
**Do** be DRY (Don't Repeat Yourself) (Evite ser redundante).

**Avoid** Evite estar tan DRY que sacrifique la legibilidad.

**¿Por qué?** Estar DRY es importante, pero no crucial si sacrifica los otros elementos de LIFT. Por eso se llama T-DRY. Por ejemplo, es redundante nombrar una plantilla hero-view.component.html porque con la extensión .html, obviamente es una vista. Pero si algo no es obvio o se aleja de una convención, entonces explíquelo.


# Pautas estructurales generales
### Estilo 04-06
**Do** Comience con poco, pero tenga en cuenta hacia dónde se dirige la aplicación.

**Do** Tenga una visión a corto plazo de la implementación y una visión a largo plazo.

**Do** Coloque todo el código de la aplicación en una carpeta llamada src.

**Considere** crear una carpeta para un componente cuando tenga varios archivos de acompañamiento (.ts, .html, .css y .spec).

**¿Por qué?**  Ayuda a mantener la estructura de la aplicación pequeña y fácil de mantener en las primeras etapas, a la vez que es fácil de evolucionar a medida que la aplicación crece.

**¿Por qué?** Los componentes a menudo tienen cuatro archivos (por ejemplo, * .html, * .css, * .ts y * .spec.ts) y pueden saturar una carpeta rápidamente.

Aquí hay una carpeta y estructura de archivos compatibles:

<pre>
+<project root>
|--+src
|--|-+app
|--|-|-+core
|--|-|-|--exception.service.ts|spec.ts
|--|-|-|--user-profile.service.ts|spec.ts
|--|-|-+heroes
|--|-|-|-+hero
|--|-|-|-|--hero.component.ts|html|css|spec.ts
|--|-|-|-+hero-list
|--|-|-|-|--hero-list.component.ts|html|css|spec.ts
|--|-|-|-+shared
|--|-|-|-|--hero-button.component.ts|html|css|spec.ts
|--|-|-|-|--hero.model.ts
|--|-|-|-|--hero.service.ts|spec.ts
|--|-|-|--heroes.component.ts|html|css|spec.ts
|--|-|-|--heroes.module.ts
|--|-|-|--heroes-routing.module.ts
|--|-|-+shared
|--|-|-|--shared.module.ts
|--|-|-|--init-caps.pipe.ts|spec.ts
|--|-|-|--filter-text.component.ts|spec.ts
|--|-|-|--filter-text.service.ts|spec.ts
|--|-|-+villains
|--|-|-|-+villain
|--|-|-|-|--...
|--|-|-|-+villain-list
|--|-|-|-|--...
|--|-|-|-+shared
|--|-|-|-|--...
|--|-|-|--villains.component.ts|html|css|spec.ts
|--|-|-|--villains.module.ts
|--|-|-|--villains-routing.module.ts
|--|-|-|--app.component.ts|html|css|spec.ts
|--|-|-|--app.module.ts
|--|-|-|--app-routing.module.ts
|--|--main.ts
|--|--index.html
|--|--...
|--node_modules/...
|--...

</pre>



>Si bien los componentes en carpetas dedicadas son ampliamente preferidos, otra opción para aplicaciones pequeñas es mantener los componentes planos (no en una carpeta dedicada). Esto agrega hasta cuatro archivos a la carpeta existente, pero también reduce el anidamiento de la carpeta. Lo que elija, sea consistente.


## Estructura de folders por funcionalidad
### Estilo 04-07
**Do** Cree carpetas con el nombre del área de características que representan.

**¿Por qué?** Un desarrollador puede localizar el código e identificar lo que cada archivo representa de un vistazo. La estructura es tan plana como puede ser y no hay nombres repetitivos o redundantes.

**¿Por qué?** Las pautas de LIFT están todas cubiertas.

**¿Por qué?** Ayuda a evitar que la aplicación se desordene organizando el contenido y manteniéndolo alineado con las pautas de LIFT.

**¿Por qué?** Cuando hay muchos archivos, por ejemplo 10+, localizarlos es más fácil con una estructura de carpetas consistente y más difícil en una estructura plana.

**Do** Cree un NgModule para cada área de características.

**¿Por qué?** hacer NgModules facilita la carga diferida de funciones enrutables.

**¿Por qué?** Hacer NgModules facilita el aislamiento, la prueba y la reutilización de funciones.


## Módulo raíz de la aplicación
### Estilo 04-08
**Do** Cree un NgModule en la carpeta raíz de la aplicación, por ejemplo, en / src / app.

**¿Por qué?** Cada aplicación requiere al menos un NgModule raíz.

**Considere** nombrar el módulo raíz app.module.ts.

**¿Por qué?** Facilita la localización e identificación del módulo raíz.

app / app.module.ts
```javascript
importar {NgModule} desde '@ angular / core';
importar {BrowserModule} desde '@ angular / platform-browser';

importar {AppComponent} desde './app.component';
importar {HeroesComponent} desde './heroes/heroes.component';

@NgModule ({
  importaciones: [
    BrowserModule,
  ],
  declaraciones: [
    AppComponent,
    HéroesComponente
  ],
  exportaciones: [AppComponent],
  entryComponents: [AppComponent]
})
clase de exportación AppModule {}
```


## Módulos de Elementos
### Estilo 04-09

**Do** Cree un NgModule para todas las características distintas en una aplicación; por ejemplo, una característica de Hero.

**Do** Coloque el módulo de funciones en la misma carpeta con nombre que el área de funciones; por ejemplo, en app / heroes.

**Do** Asigne un nombre al archivo del módulo de funciones que refleje el nombre del área de funciones y la carpeta; por ejemplo, app / heroes / heroes.module.ts.

**Do** Nombra la firma del módulo de características reflejando el nombre del área de funciones, carpeta y archivo; por ejemplo, app / heroes / heroes.module.ts define HeroesModule.

**¿Por qué?** Un módulo de características puede exponer u ocultar su implementación de otros módulos.

**¿Por qué?** Un módulo de características identifica conjuntos distintos de componentes relacionados que comprenden el área de características.

**¿Por qué?** Un módulo de funciones se puede enrutar fácilmente eagerly o lazily.

**¿Por qué?** Un módulo de características define límites claros entre la funcionalidad específica y otras características de la aplicación.

**¿Por qué?** Un módulo de funciones ayuda a aclarar y facilitar la asignación de responsabilidades de desarrollo a diferentes equipos.

**¿Por qué?** Un módulo de funciones se puede aislar fácilmente para realizar pruebas.


## Módulo de características compartidas
### Estilo 04-10
**Do** Cree un módulo de funciones denominado SharedModule en una carpeta compartida; por ejemplo, app / shared / shared.module.ts define SharedModule.

**Do** Declare componentes, directivas y canalizaciones en un módulo compartido cuando esos elementos serán reutilizados y referenciados por los componentes declarados en otros módulos de características.

**Considere** usar el nombre SharedModule cuando se hace referencia a los contenidos de un módulo compartido en toda la aplicación.

**Considere** no proporcionar servicios en módulos compartidos. Los servicios suelen ser singletons que se proporcionan una vez para toda la aplicación o en un módulo de características en particular. Hay excepciones, sin embargo. Por ejemplo, en el código de muestra que sigue, observe que SharedModule proporciona FilterTextService. Esto es aceptable aquí porque el servicio no tiene estado; es decir, los consumidores del servicio no se ven afectados por nuevas instancias.

**Do** Importe todos los módulos requeridos por los activos en SharedModule; por ejemplo, CommonModule y FormsModule.

**¿Por qué?** SharedModule contendrá componentes, directivas y tuberías que pueden necesitar características de otro módulo común; por ejemplo, ngFor en CommonModule.

**Do** Declare todos los componentes, directivas y tuberías en SharedModule.

**Do** Exporte todos los símbolos del SharedModule que otros módulos de características deben usar.

**¿Por qué?** SharedModule existe para hacer que los componentes, directivas y tuberías de uso común estén disponibles para su uso en las plantillas de componentes en muchos otros módulos.

**Avoid** Evite especificar proveedores singleton para toda la aplicación en un SharedModule. Los singletons ocacionales están bien. tenga cuidado.

**¿Por qué?** Un módulo de funciones con lazy load que importa ese módulo compartido hará su propia copia del servicio y probablemente tendrá resultados no deseados.

**¿Por qué?** No desea que cada módulo tenga su propia instancia separada de servicios singleton. Sin embargo, existe un peligro real de que eso suceda si SharedModule proporciona un servicio.

<pre>
+src
|--+app
|--|--+shared
|--|--|--shared.module.ts
|--|--|--init-caps.pipe.ts|spec.ts
|--|--|--filter-text.component.ts|spec.ts
|--|--|--filter-text.service.ts|spec.ts
|--|--app.component.ts|html|css|spec.ts
|--|--app.module.ts
|--|--app-routing.module.ts
|--main.ts
|--index.html
...
</pre>

>app/shared/shared.module.ts
```javascript
import { NgModule }      from '@angular/core';
import { CommonModule }  from '@angular/common';
import { FormsModule }   from '@angular/forms';

import { FilterTextComponent } from './filter-text/filter-text.component';
import { FilterTextService }   from './filter-text/filter-text.service';
import { InitCapsPipe }        from './init-caps.pipe';

@NgModule({
  imports: [CommonModule, FormsModule],
  declarations: [
    FilterTextComponent,
    InitCapsPipe
  ],
  providers: [FilterTextService],
  exports: [
    CommonModule,
    FormsModule,
    FilterTextComponent,
    InitCapsPipe
  ]
})
export class SharedModule { }
```
>app/shared/init-caps.pipe.ts
```javascript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'initCaps' })
export class InitCapsPipe implements PipeTransform {
  transform = (value: string) => value;
}

``
>app/shared/filter-text/filter-text.component.ts
```javascript
import { Component, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'toh-filter-text',
  template: '<input type="text" id="filterText" [(ngModel)]="filter" (keyup)="filterChanged($event)" />'
})
export class FilterTextComponent {
  @Output() changed: EventEmitter<string>;

  filter: string;

  constructor() {
    this.changed = new EventEmitter<string>();
  }

  clear() {
    this.filter = '';
  }

  filterChanged(event: any) {
    event.preventDefault();
    console.log(`Filter Changed: ${this.filter}`);
    this.changed.emit(this.filter);
  }
}
```
>app/shared/filter-text/filter-text.service.ts
```javascript
import { Injectable } from '@angular/core';

@Injectable()
export class FilterTextService {
  constructor() {
    console.log('Created an instance of FilterTextService');
  }

  filter(data: string, props: Array<string>, originalList: Array<any>) {
    let filteredList: any[];
    if (data && props && originalList) {
      data = data.toLowerCase();
      let filtered = originalList.filter(item => {
        let match = false;
        for (let prop of props) {
          if (item[prop].toString().toLowerCase().indexOf(data) > -1) {
            match = true;
            break;
          }
        };
        return match;
      });
      filteredList = filtered;
    } else {
      filteredList = originalList;
    }
    return filteredList;
  }
}
```
>app/heroes/heroes.component.ts
```javascript
import { Component } from '@angular/core';

import { FilterTextService } from '../shared/filter-text/filter-text.service';

@Component({
  selector: 'toh-heroes',
  templateUrl: './heroes.component.html'
})
export class HeroesComponent {

  heroes = [
    { id: 1, name: 'Windstorm' },
    { id: 2, name: 'Bombasto' },
    { id: 3, name: 'Magneta' },
    { id: 4, name: 'Tornado' }
  ];

  filteredHeroes = this.heroes;

  constructor(private filterService: FilterTextService) { }

  filterChanged(searchText: string) {
    this.filteredHeroes = this.filterService.filter(searchText, ['id', 'name'], this.heroes);
  }
}
```
>app/heroes/heroes.component.html
```html
<div>This is heroes component</div>
<ul>
  <li *ngFor="let hero of filteredHeroes">
    {{hero.name}}
  </li>
</ul>
<toh-filter-text (changed)="filterChanged($event)"></toh-filter-text>
```



## Carpetas de carga perezosa (Lazy load)
### Estilo 04-11
Una característica de aplicación o flujo de trabajo distinto puede cargarse de forma diferida o cargarse a pedido en lugar de cuando se inicia la aplicación.

**Do** Ponga el contenido de las funciones cargadas diferidas en una carpeta cargada diferida. Una carpeta típica con carga lenta contiene un componente de enrutamiento, sus componentes secundarios y sus activos y módulos relacionados.

**¿Por qué?** La carpeta facilita la identificación y el aislamiento del contenido de la función.


## Nunca importe directamente carpetas de carga perezosa (Lazy load)
### Estilo 04-12
Evite permitir que los módulos en las carpetas de hermanos y padres importen directamente un módulo en una función de carga diferida.

**¿Por qué?** La importación directa y el uso de un módulo lo cargarán inmediatamente cuando la intención sea cargarlo a pedido.


### Componentes
#### Componentes como elementos
##### Estilo 05-03
**Considere** dar a los componentes un selector de elementos, en oposición a los selectores de atributos o clases.

**¿Por qué?** Los componentes tienen plantillas que contienen HTML y sintaxis de plantilla angular opcional. Muestran contenido. Los desarrolladores colocan componentes en la página como lo harían con elementos HTML nativos y componentes web.

**¿Por qué?** Es más fácil reconocer que un símbolo es un componente mirando el html de la plantilla.

>Hay algunos casos en los que le da un atributo a un componente, como cuando desea aumentar un elemento incorporado. Por ejemplo, Material Design usa esta técnica con *\<button mat-button>*. Sin embargo, no usaría esta técnica en un elemento personalizado.

app / heroes / hero-button / hero-button.component.ts

> *evitar*
```javascript
/* avoid */

@Component({
  selector: '[tohHeroButton]',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent {}
```

app/app.component.html
>*evitar*
```html
<!-- avoid -->

<div tohHeroButton></div>
```
> **Lo correcto**
app/heroes/shared/hero-button/hero-button.component.ts
```javascript
@Component({
  selector: 'toh-hero-button',
  templateUrl: './hero-button.component.html'
})
export class HeroButtonComponent {}
```
app/app.component.html
```html
<toh-hero-button></toh-hero-button>
```

### Extraer plantillas y estilos en sus propios archivos.
#### Estilo 05-04
**Do** Extraiga plantillas y estilos en un archivo separado, cuando haya más de 3 líneas.

**Do** Nombre el archivo de plantilla [nombre-componente] .component.html, donde [nombre-componente] es el nombre del componente.

**Do** Nombre el archivo de estilo [nombre-componente] .component.css, donde [nombre-componente] es el nombre del componente.

**Do** Especifique las URL relativas a los componentes, con el prefijo ./.

**¿Por qué?** Las plantillas y estilos grandes en línea oscurecen el propósito y la implementación del componente, reduciendo la legibilidad y la facilidad de mantenimiento.

**¿Por qué?** En la mayoría de los editores, las sugerencias de sintaxis y los fragmentos de código no están disponibles al desarrollar plantillas y estilos en línea. Angular TypeScript Language Service (de próxima aparición) promete superar esta deficiencia para las plantillas HTML en aquellos editores que lo admitan; No ayudará con los estilos CSS.

**¿Por qué?** La URL relativa de un componente no requiere cambios cuando mueve los archivos de componentes, siempre que los archivos permanezcan juntos.

**¿Por qué?** El prefijo ./ es una sintaxis estándar para las URL relativas; no dependa de la capacidad actual de Angular para prescindir de ese prefijo.

> *Evitar*

app / heroes / heroes.component.ts

```javascript
/* avoid */

@Component({
  selector: 'toh-heroes',
  template: `
    <div>
      <h2>My Heroes</h2>
      <ul class="heroes">
        <li *ngFor="let hero of heroes | async" (click)="selectedHero=hero">
          <span class="badge">{{hero.id}}</span> {{hero.name}}
        </li>
      </ul>
      <div *ngIf="selectedHero">
        <h2>{{selectedHero.name | uppercase}} is my hero</h2>
      </div>
    </div>
  `,
  styles: [`
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .heroes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .heroes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color: #607D8B;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
  `]
})
export class HeroesComponent implements OnInit {
  heroes: Observable<Hero[]>;
  selectedHero: Hero;

 constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.heroes = this.heroService.getHeroes();
  }
}
```
> En lugar de eso
app/heroes/heroes.component.ts
```javascript
@Component({
  selector: 'toh-heroes',
  templateUrl: './heroes.component.html',
  styleUrls:  ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes: Observable<Hero[]>;
  selectedHero: Hero;

 constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.heroes = this.heroService.getHeroes();
  }
}
```
app/heroes/heroes.component.html
```html
<div>
  <h2>My Heroes</h2>
  <ul class="heroes">
    <li *ngFor="let hero of heroes | async" (click)="selectedHero=hero">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </li>
  </ul>
  <div *ngIf="selectedHero">
    <h2>{{selectedHero.name | uppercase}} is my hero</h2>
  </div>
</div>
```
app/heroes/heroes.component.css
```css
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #607D8B;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
```

### Decorar propiedades de entrada y salida (Añadir anotaciones)
#### Estilo 05-12
**Do** Utilice los decoradores de clase @Input () y @Output () en lugar de las propiedades de entradas y salidas de los metadatos @Directive y @Component:

**Considere*** colocar @Input () o @Output () en la misma línea que la propiedad que decora.

**¿Por qué?** Es más fácil y más legible identificar qué propiedades en una clase son entradas o salidas.

**¿Por qué?** Si alguna vez necesita cambiar el nombre de la propiedad o el nombre del evento asociado con @Input o @Output, puede modificarlo en un solo lugar.

**¿Por qué?** La declaración de metadatos adjunta a la directiva es más corta y, por lo tanto, más legible.

**¿Por qué?** Colocar el decorador en la misma línea generalmente hace que el código sea más corto y aún así identifica fácilmente la propiedad como una entrada o salida. Póngalo en la línea de arriba cuando hacerlo sea claramente más legible.

> *Evitar*

app / heroes / shared / hero-button / hero-button.component.ts
```javascript
/* avoid */

@Component({
  selector: 'toh-hero-button',
  template: `<button></button>`,
  inputs: [
    'label'
  ],
  outputs: [
    'change'
  ]
})
export class HeroButtonComponent {
  change = new EventEmitter<any>();
  label: string;
}
```
> *En su lugar*

app/heroes/shared/hero-button/hero-button.component.ts
```javascript
@Component({
  selector: 'toh-hero-button',
  template: `<button>{{label}}</button>`
})
export class HeroButtonComponent {
  @Output() change = new EventEmitter<any>();
  @Input() label: string;
}
```

### Evite el alias de *inputs and outputs*
#### Estilo 05-13
**Do** Evite los alias de entrada y salida, excepto cuando tenga un propósito importante.

**¿Por qué?** Dos nombres para la misma propiedad (uno privado, uno público) es intrínsecamente confuso.

**¿Por qué?** Debe usar un alias cuando el nombre de la directiva también sea una propiedad de entrada y el nombre de la directiva no describa la propiedad.

> *Evitar*

app / heroes / shared / hero-button / hero-button.component.ts

```javascript
/* avoid pointless aliasing */

@Component({
  selector: 'toh-hero-button',
  template: `<button>{{label}}</button>`
})
export class HeroButtonComponent {
  // Pointless aliases
  @Output('changeEvent') change = new EventEmitter<any>();
  @Input('labelAttribute') label: string;
}
```
app/app.component.html
```html
<!-- avoid -->

<toh-hero-button labelAttribute="OK" (changeEvent)="doSomething()">
</toh-hero-button>
```
>**En su lugar**

app/heroes/shared/hero-button/hero-button.component.ts
```javascript
@Component({
  selector: 'toh-hero-button',
  template: `<button>{{label}}</button>`
})
export class HeroButtonComponent {
  // No aliases
  @Output() change = new EventEmitter<any>();
  @Input() label: string;
}
```
app/heroes/shared/hero-button/hero-highlight.directive.ts
```javascript
import { Directive, ElementRef, Input, OnChanges } from '@angular/core';

@Directive({ selector: '[heroHighlight]' })
export class HeroHighlightDirective implements OnChanges {

  // Aliased because `color` is a better property name than `heroHighlight`
  @Input('heroHighlight') color: string;

  constructor(private el: ElementRef) {}

  ngOnChanges() {
    this.el.nativeElement.style.backgroundColor = this.color || 'yellow';
  }
}
```
app/app.component.html
```html
<toh-hero-button label="OK" (change)="doSomething()">
</toh-hero-button>

<!-- `heroHighlight` is both the directive name and the data-bound aliased property name -->
<h3 heroHighlight="skyblue">The Great Bombasto</h3>

```

### Secuencia de miembros
#### Estilo 05-14
**Do** Coloque las propiedades en la parte superior seguido de los métodos.

**Do** Coloque a los miembros privados después de los miembros públicos, ordenados alfabéticamente.

**¿Por qué?** Colocar a los miembros en una secuencia consistente hace que sea fácil de leer y ayuda a identificar instantáneamente qué miembros del componente sirven para qué propósito.

>*Evitar*

app / shared / toast / toast.component.ts
```javascript
/* avoid */

export class ToastComponent implements OnInit {

  private defaults = {
    title: '',
    message: 'May the Force be with you'
  };
  message: string;
  title: string;
  private toastElement: any;

  ngOnInit() {
    this.toastElement = document.getElementById('toh-toast');
  }

  // private methods
  private hide() {
    this.toastElement.style.opacity = 0;
    window.setTimeout(() => this.toastElement.style.zIndex = 0, 400);
  }

  activate(message = this.defaults.message, title = this.defaults.title) {
    this.title = title;
    this.message = message;
    this.show();
  }

  private show() {
    console.log(this.message);
    this.toastElement.style.opacity = 1;
    this.toastElement.style.zIndex = 9999;

    window.setTimeout(() => this.hide(), 2500);
  }
}
```

> *En su lugar*
app/shared/toast/toast.component.ts
```javascript
export class ToastComponent implements OnInit {
  // public properties
  message: string;
  title: string;

  // private fields
  private defaults = {
    title: '',
    message: 'May the Force be with you'
  };
  private toastElement: any;

  // public methods
  activate(message = this.defaults.message, title = this.defaults.title) {
    this.title = title;
    this.message = message;
    this.show();
  }

  ngOnInit() {
    this.toastElement = document.getElementById('toh-toast');
  }

  // private methods
  private hide() {
    this.toastElement.style.opacity = 0;
    window.setTimeout(() => this.toastElement.style.zIndex = 0, 400);
  }

  private show() {
    console.log(this.message);
    this.toastElement.style.opacity = 1;
    this.toastElement.style.zIndex = 9999;
    window.setTimeout(() => this.hide(), 2500);
  }
}
```

### Delegar lógica de componentes complejos a servicios
#### Estilo 05-15
**Do*** Limite la lógica en un componente solo a la requerida para la vista. Toda otra lógica debe delegarse a los servicios.

**Do** Mueva la lógica reutilizable a los servicios y mantenga los componentes simples y enfocados en su propósito previsto.

**¿Por qué?** La lógica puede ser reutilizada por múltiples componentes cuando se coloca dentro de un servicio y se expone a través de una función.

**¿Por qué?** La lógica en un servicio se puede aislar más fácilmente en una prueba unitaria, mientras que la lógica de llamada en el componente se puede burlar fácilmente.

**¿Por qué?** Elimina dependencias y oculta detalles de implementación del componente.

**¿Por qué?** Mantiene el componente delgado, recortado y enfocado.

>*Evitar*

app / heroes / hero-list / hero-list.component.ts
```javascript
/* avoid */

import { OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

import { Observable } from 'rxjs';
import { catchError, finalize } from 'rxjs/operators';

import { Hero } from '../shared/hero.model';

const heroesUrl = 'http://angular.io';

export class HeroListComponent implements OnInit {
  heroes: Hero[];
  constructor(private http: HttpClient) {}
  getHeroes() {
    this.heroes = [];
    this.http.get(heroesUrl).pipe(
      catchError(this.catchBadResponse),
      finalize(() => this.hideSpinner())
    ).subscribe((heroes: Hero[]) => this.heroes = heroes);
  }
  ngOnInit() {
    this.getHeroes();
  }

  private catchBadResponse(err: any, source: Observable<any>) {
    // log and handle the exception
    return new Observable();
  }

  private hideSpinner() {
    // hide the spinner
  }
}
```

No prefijas las propiedades de salida
Estilo 05-16
Nombre los eventos sin el prefijo activado.

Nombre los métodos del controlador de eventos con el prefijo seguido del nombre del evento.

¿Por qué? Esto es coherente con los eventos integrados, como los clics en los botones.

¿Por qué? Angular permite una sintaxis alternativa en- *. Si el evento en sí tenía el prefijo on, esto daría como resultado una expresión de enlace on-onEvent.

app / heroes / hero.component.ts
content_copy
/ * evitar * /

@Componente({
  selector: 'toh-hero',
  plantilla: `...`
})
export class HeroComponent {
  @Output () onSavedTheDay = new EventEmitter <boolean> ();
}
app / app.component.html
content_copy
<! - evitar ->

<toh-hero (onSavedTheDay) = "onSavedTheDay ($ event)"> </toh-hero>
app / heroes / hero.component.ts
app / app.component.html
content_copy
<toh-hero (savedTheDay) = "onSavedTheDay ($ event)"> </toh-hero>
Volver arriba

Poner lógica de presentación en la clase de componente
Estilo 05-17
Ponga la lógica de presentación en la clase de componente, y no en la plantilla.

¿Por qué? La lógica estará contenida en un lugar (la clase de componente) en lugar de extenderse en dos lugares.

¿Por qué? Mantener la lógica de presentación del componente en la clase en lugar de la plantilla mejora la capacidad de prueba, la capacidad de mantenimiento y la reutilización.

app / heroes / hero-list / hero-list.component.ts
content_copy
/ * evitar * /

@Componente({
  selector: 'toh-hero-list',
  plantilla: `
    <sección>
      Nuestra lista de héroes:
      <hero-profile * ngFor = "let hero of heroes" [hero] = "hero">
      </hero-profile>
      Potencias totales: {{totalPowers}} <br>
      Poder promedio: {{totalPowers / heroes.length}}
    </section>
  ``
})
export class HeroListComponent {
  héroes: Héroe [];
  totalPowers: número;
}
app / heroes / hero-list / hero-list.component.ts
content_copy
@Componente({
  selector: 'toh-hero-list',
  plantilla: `
    <sección>
      Nuestra lista de héroes:
      <toh-hero * ngFor = "let hero of heroes" [hero] = "hero">
      </toh-hero>
      Potencias totales: {{totalPowers}} <br>
      Potencia media: {{avgPower}}
    </section>
  ``
})
export class HeroListComponent {
  héroes: Héroe [];
  totalPowers: número;

  obtener avgPower () {
    devuelve this.totalPowers / this.heroes.length;
  }
}
Volver arriba

Directivas
Usar directivas para mejorar un elemento
Estilo 06-01
Use directivas de atributos cuando tenga una lógica de presentación sin una plantilla.

¿Por qué? Las directivas de atributos no tienen una plantilla asociada.

¿Por qué? Un elemento puede tener más de una directiva de atributo aplicada.

app / shared / highlight.directive.ts
content_copy
@Directiva({
  selector: '[tohHighlight]'
})
clase de exportación HighlightDirective {
  @HostListener ('mouseover') onMouseEnter () {
    // destaca el trabajo
  }
}
app / app.component.html
content_copy
<div tohHighlight> Bombasta </div>
Volver arriba

Decoradores HostListener / HostBinding versus metadatos del host
Estilo 06-03
Considere preferir @HostListener y @HostBinding a la propiedad de host de @Di
decoradores reactivos y @Component.

Sea consistente en su elección.

¿Por qué? La propiedad asociada con @HostBinding o el método asociado con @HostListener solo se puede modificar en un solo lugar: en la clase de la directiva. Si utiliza la propiedad de metadatos del host, debe modificar tanto la declaración de propiedad / método en la clase de la directiva como los metadatos en el decorador asociado con la directiva.

app / shared / validator.directive.ts
content_copy
importar {Directiva, HostBinding, HostListener} desde '@ angular / core';

@Directiva({
  selector: '[tohValidator]'
})
export class ValidatorDirective {
  @HostBinding ('attr.role') role = 'button';
  @HostListener ('mouseenter') onMouseEnter () {
    // hacer trabajo
  }
}
Compare con la alternativa de metadatos de host menos preferida.

¿Por qué? Los metadatos del host son solo un término para recordar y no requieren importaciones adicionales de ES.

app / shared / validator2.directive.ts
content_copy
importar {Directiva} desde '@ angular / core';

@Directiva({
  selector: '[tohValidator2]',
  anfitrión: {
    '[attr.role]': 'rol',
    '(mouseenter)': 'onMouseEnter ()'
  }
})
export class Validator2Directive {
  rol = 'botón';
  onMouseEnter () {
    // hacer trabajo
  }
}
Volver arriba

Servicios
Los servicios son singletons
Estilo 07-01
Utilice los servicios como singletons dentro del mismo inyector. Úselos para compartir datos y funcionalidad.

¿Por qué? Los servicios son ideales para compartir métodos en un área de características o una aplicación.

¿Por qué? Los servicios son ideales para compartir datos en memoria con estado.

app / heroes / shared / hero.service.ts
content_copy
export class HeroService {
  constructor (http privado: HttpClient) {}

  getHeroes () {
    devuelve this.http.get <Hero []> ('api / heroes');
  }
}
Volver arriba

Responsabilidad única
Estilo 07-02
Cree servicios con una única responsabilidad que esté encapsulada por su contexto.

Cree un nuevo servicio una vez que el servicio comience a exceder ese propósito singular.

¿Por qué? Cuando un servicio tiene múltiples responsabilidades, se hace difícil probarlo.

¿Por qué? Cuando un servicio tiene múltiples responsabilidades, cada componente o servicio que lo inyecta ahora tiene el peso de todos ellos.

Volver arriba

Brindar un servicio
Estilo 07-03
Proporcione un servicio con el inyector raíz de la aplicación en el decorador @Injectable del servicio.

¿Por qué? El inyector angular es jerárquico.

¿Por qué? Cuando proporciona el servicio a un inyector raíz, esa instancia del servicio se comparte y está disponible en todas las clases que lo necesitan. Esto es ideal cuando un servicio comparte métodos o estados.

¿Por qué? Cuando registra un servicio en el decorador @Injectable del servicio, las herramientas de optimización como las utilizadas por las compilaciones de producción de Angular CLI pueden realizar sacudidas de árboles y eliminar servicios que su aplicación no utiliza.

¿Por qué? Esto no es ideal cuando dos componentes diferentes necesitan instancias diferentes de un servicio. En este escenario, sería mejor proporcionar el servicio en el nivel de componente que necesita la instancia nueva y separada.

src / app / treeshaking / service.ts
content_copy
@Inyectable ({
  provideIn: 'root',
})
Servicio de clase de exportación {
}
Volver arriba

Use el decorador de clase @Injectable ()
Estilo 07-04
Utilice el decorador de clase @Injectable () en lugar del decorador de parámetros @Inject cuando utilice tipos como tokens para las dependencias de un servicio.

¿Por qué? El mecanismo de inyección de dependencia angular (DI) resuelve las dependencias propias de un servicio en función de los tipos declarados de los parámetros de constructor de ese servicio.

¿Por qué? Cuando un servicio acepta solo dependencias asociadas con tokens de tipo, la sintaxis @Injectable () es mucho menos detallada en comparación con el uso de @Inject () en cada parámetro de constructor individual.

app / heroes / shared / hero-arena.service.ts
content_copy
/ * evitar * /

clase de exportación HeroArena {
  constructor(
      @Inject (HeroService) heroService privado: HeroService,
      @Inject (HttpClient) http privado: HttpClient) {}
}
app / heroes / shared / hero-arena.service.ts
content_copy
@Inyectable ()
clase de exportación HeroArena {
  constructor(
    heroService privado: HeroService,
    http privado: HttpClient) {}
}
Volver arriba

Servicios de datos
Hable con el servidor a través de un servicio.
Estilo 08-01
Refactorice la lógica para realizar operaciones de datos e interactuar con datos a un servicio.

Haga que los servicios de datos sean responsables de las llamadas XHR, el almacenamiento local, el almacenamiento en la memoria o cualquier otra operación de datos.

¿Por qué? La responsabilidad del componente es la presentación y recopilación de información para la vista. No debería importarle cómo obtiene los datos, solo que sabe a quién pedirlos. La separación de los servicios de datos mueve la lógica sobre cómo llevarlos al servicio de datos, y permite que el componente sea más simple y más centrado en la vista.

¿Por qué? Esto hace que sea más fácil probar (simulacro o real) las llamadas de datos al probar un componente que utiliza un servicio de datos.

¿Por qué? Los detalles de la gestión de datos, como encabezados, métodos HTTP, almacenamiento en caché, manejo de errores y lógica de reintento, son irrelevantes para los componentes y otros consumidores de datos.

Un servicio de datos encapsula estos detalles. Es más fácil evolucionar estos detalles dentro del servicio consin afectar a sus consumidores. Y es más fácil probar a los consumidores con implementaciones de servicios simulados.

Volver arriba

Ganchos de ciclo de vida
Use los ganchos de Lifecycle para aprovechar eventos importantes expuestos por Angular.

Volver arriba

Implementar interfaces de enlace de ciclo de vida
Estilo 09-01
Implemente las interfaces de enlace de ciclo de vida.

¿Por qué? Las interfaces del ciclo de vida prescriben firmas de métodos mecanografiados. Use esas firmas para marcar errores de ortografía y sintaxis.

app / heroes / shared / hero-button / hero-button.component.ts
content_copy
/ * evitar * /

@Componente({
  selector: 'toh-hero-button',
  plantilla: `<botón> OK <botón>`
})
export class HeroButtonComponent {
  onInit () {// mal escrito
    console.log ('El componente se inicializa');
  }
}
app / heroes / shared / hero-button / hero-button.component.ts
content_copy
@Componente({
  selector: 'toh-hero-button',
  plantilla: `<button> OK </button>`
})
export class HeroButtonComponent implementa OnInit {
  ngOnInit () {
    console.log ('El componente se inicializa');
  }
}
Volver arriba

Apéndice
Herramientas y consejos útiles para Angular.

Volver arriba

Codelyzer
Estilo A-01
Use codelyzer para seguir esta guía.

Considere ajustar las reglas en codelyzer para satisfacer sus necesidades.

Volver arriba

Plantillas de archivo y fragmentos
Estilo A-02
Utilice plantillas de archivo o fragmentos para ayudar a seguir estilos y patrones consistentes. Aquí hay plantillas y / o fragmentos para algunos de los editores de desarrollo web e IDE.

Considere usar fragmentos para Visual Studio Code que sigan estos estilos y pautas.

Usar extensión
Considere usar fragmentos para Atom que sigan estos estilos y pautas.

Considere usar fragmentos para Sublime Text que sigan estos estilos y pautas.

Considere usar fragmentos para Vim que sigan estos estilos y pautas.

