# geolocationApp
## App en ionic para el meetUp de MedellinJS

Importante: antes de configurar el ambiente, asegurate que tengas instalado npm, si no lo tienes instalado descargalo 
del siguiente link y descarga la versión LTS:

https://nodejs.org/es/

## Instalación ambiente:

Ingresamos a la terminal (si estas en windows ingresa a la terminal de node no la cmd) y seguimos los siguientes pasos:

1. digitamos el siguiente comando:
* npm install -g cordova ionic

## Crear una app 

digitamos en la cosola el siguiente comando:
* ionic start geolocationApp blank

# Ejecutar la app:

digitamos en la cosola los siguiente comandos:
* cd geolocationApp
* ionic serv

# Abrimos el proyecto:
* Recomiendo Visual Studio Code, pero igual puedes usar su IDE de preferencia.

# Creamos una nueva página:
* ionic generate page map
* ingresamos al archivo **app.module.ts** y agregamos la siguiente linea de código en los imports:
  ````javascript
  import { MapPage } from '../pages/map/map';
  
* Reeemplazamos el **@NgModel** por el siguiente código:
  ````javascript
  @NgModule({
  declarations: [
    MyApp,
    MapPage
  ],
  imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    MapPage
  ],
  providers: [
    StatusBar,
    SplashScreen,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})

* En el archivo **app.component.ts** agregamos el siguiente import:
  ````javascript
  import { MapPage } from '../pages/map/map';

* y reemplazamos la variable **rootPage** por:
  ````javascript
  rootPage:any = MapPage;
  
* es opcional (para este taller) eliminar el archivo **map.module.ts**

* ingresamos a la documentación de GoogleMaps para ionic en la url:
  https://ionicframework.com/docs/native/google-maps/
  
* instalamos el paquete de GoogleMaps con el siguiente comando en consola:
  npm install --save @ionic-native/google-maps
  
* abrimos el archivo **map.ts** y agregamos los siguientes imports:
  ````javascript
  import { GoogleMaps, GoogleMap, GoogleMapsEvent, LatLng } from '@ionic-native/google-maps';
  import { Geolocation } from '@ionic-native/geolocation';
  
* en el decorador **@Component** agregamos el provider:
  ````javascript
  providers: [GoogleMaps, Geolocation]
  
* en la clase **MapPage** agregamos la variable publica **map**:
  ````javascript
  public map: GoogleMap;
  
* en el constructor agregamos los parametros de GoogleMaps:
  ````javascript
  constructor(public navCtrl: NavController, 
  public navParams: NavParams, 
  public googleMaps: GoogleMaps, 
  private platform: Platform, 
  private geolocation: Geolocation) {}
  
* luego en la clase **ionViewDidLoad()** insertamos el siguiente código:
  ````javascript
  ionViewDidLoad() {
    console.log('ionViewDidLoad MapPage');
    this.platform.ready().then(() => {
        let element: HTMLElement = document.getElementById('map');
        this.map = this.googleMaps.create(element);
        this.map.one(GoogleMapsEvent.MAP_READY).then((data:any) => {
          this.geolocation.getCurrentPosition().then(pos => {
            let myPosition = new LatLng(pos.coords.latitude, pos.coords.longitude);
            this.map.animateCamera({target: myPosition, zoom: 10});
            this.map.addMarker({
              position: myPosition,
              title: 'You are here'
            });
          });
        });
    });
  }
  
* en el archivo **map.html** dentro de la etiqueta **<ion-content>** insertamos el siguiente código:
  ```javascript
  <div id="map"></div>

* en el archivo **map.scss** agragamos las siguientes lineas de código:
  ````javascript
  #map {
    height: 100%;
    width: 101%;
  }

  ion-app._gmaps_cdv_ .nav-decor {
      display: none !important;
  }
  
# Creación de las API's Keys de GoogleMaps:

* ingresamos a la siguiente url:
  https://console.developers.google.com

* luego creamos un nuevo proyecto, le ponemos el nombre de geolocationApp

* habilitamos el GoogleMaps Android API y GoogleMaps SDK para ios

* seleccionamos el botón de **Habilitar**

* vamos a las **Credenciales** en el menú derecho del panel

* damos clic en **Crear credenciales*

* seleccionamos **Clave de API**

* y por último copiamos la clave que nos genera

# Instalacion del plugin de Cordova para las claves de API de GoogleMaps

* ejecutamos en consola el siguiente comando:

  ionic cordova plugin add cordova-plugin-googlemaps --variable API_KEY_FOR_ANDROID="YOUR_ANDROID_API_KEY_IS_HERE" --variable API_KEY_FOR_IOS="YOUR_IOS_API_KEY_IS_HERE"
  
* reemplazamos en **YOUR_ANDROID_API_KEY_IS_HERE** por la clave que generamos anteriormente, quedaría algo como:

  ionic cordova plugin add cordova-plugin-googlemaps --variable API_KEY_FOR_ANDROID="AIzaSyAKehETi7kRaQYHNU92-o8D2r1abbUu916"
  

# Ejecutar la app

* para ejecutar la app, para este caso lo haremos con un dispositivo android, es solo conectar el dispositivo con el
cable usb al pc, y luego ejecutamos el siguiente comando en consola:

  ionic cordova run android
  
* para compilarlo usamos el siguiente comando en consola:
  
  ionic cordova build android
  
 
# Tips:
    
    Ejecutar aplicación:
    * ionic serve
    
    Crear una nueva página:
    * ionic generate page namePage

    Añadir la plataforma android: 
    * ionic cordova platform add android 
    
    Añadir la plataforma ios: 
    * ionic cordova platform add ios 
    
    Añadir un plugin: 
    * ionic cordova plugin add nombreDelPlugin 
    
    Hacer un build en android: 
    * ionic cordova build android
    
    Hacer un build en ios: 
    * ionic cordova build ios
    
    Ejecutar la app en android:
    * ionic cordova run android
    
    Ejecutar la app en ios:
    * ionic cordova run ios
