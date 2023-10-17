# Producto 1
Azure geolocalizacion Mayra-Karlos

Este inicio rápido demuestra cómo usar Azure Maps para crear un mapa que brinde a los usuarios una experiencia de búsqueda interactiva. Lo guía a través de estos pasos básicos:

1.Cree su propia cuenta de Azure Maps.

2.Obtenga su clave de suscripción de Azure Maps para usarla en la aplicación web de demostración.

3.Descargue y abra la aplicación de mapas de demostración.


Este inicio rápido usa el SDK web de Azure Maps; sin embargo, el servicio Azure Maps se puede usar con cualquier control de mapa, como estos controles de mapas populares de código abierto para los que el equipo de Azure Maps ha creado complementos.

#Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de comenzar.

Inicie sesión en el portal de Azure .


#Crear una cuenta de Azure Maps

Cree una nueva cuenta de Azure Maps con los siguientes pasos:

1.Seleccione Crear un recurso en la esquina superior izquierda de Azure Portal .

2.Escriba Azure Maps en el cuadro Servicios de búsqueda y Marketplace .

3.Seleccione Azure Maps en la lista desplegable que aparece y luego seleccione el botón Crear .

4.En la página de recursos Crear una cuenta de Azure Maps , ingrese los siguientes valores y luego seleccione el botón Crear :

	A.La suscripción que desea utilizar para esta cuenta.
	B.El nombre del grupo de recursos para esta cuenta. Puede optar por Crear un grupo de recursos nuevo o 		Seleccionar un grupo de recursos existente .
	C.El nombre de su nueva cuenta de Azure Maps.
	D.El nivel de precios para esta cuenta. Seleccione Gen2 .
	E.Lea la Declaración de licencia y privacidad y luego seleccione la casilla de verificación para aceptar los 	términos.


#Obtenga la clave de suscripción para su cuenta

Una vez que su cuenta de Azure Maps se haya creado correctamente, recupere la clave de suscripción que le permite consultar las API de Maps.

1.Abra su cuenta de Maps en el portal.

2.En la sección de configuración, seleccione Autenticación .

3.Copie la clave principal y guárdela localmente para usarla más adelante en este tutorial.


#Descargue y actualice la demostración de Azure Maps

1.Copie el contenido del archivo: Interactive Search Quickstart.html .
2.Guarde el contenido de este archivo localmente como AzureMapDemo.html . Ábrelo en un editor de texto.
3.Agregue el valor de la clave principal que obtuvo en la sección anterior
  	A.Comente todo el código de la authOptionsfunción; este código se utiliza para la autenticación de Azure Active 
  	Directory.
	B.Descomente las dos últimas líneas de la authOptionsfunción; este código se utiliza para la autenticación de 	clave compartida, método que se utiliza en este inicio rápido.
	C.Reemplácela <Your Azure Maps Key>con el valor de la clave de suscripción de la sección anterior.

#Abra la aplicación de demostración

1.Abra el archivo AzureMapDemo.html en el navegador de su elección.

2.Observe el mapa que se muestra de la ciudad de Los Ángeles. Acerca y aleja el zoom para ver cómo el mapa se representa automáticamente con más o menos información dependiendo del nivel de zoom.

3.Cambia el centro predeterminado del mapa. En el archivo AzureMapDemo.html , busque la variable denominada centro . Reemplace el valor del par de longitud y latitud para esta variable con los nuevos valores [-74.0060, 40.7128] . Guarde el archivo y actualice su navegador.

4.Pruebe la experiencia de búsqueda interactiva. En el cuadro de búsqueda en la esquina superior izquierda de la aplicación web de demostración, busque restaurantes .

5.Mueva el mouse sobre la lista de direcciones y ubicaciones que aparecen debajo del cuadro de búsqueda. Observe cómo el pin correspondiente en el mapa muestra información sobre esa ubicación. Para garantizar la privacidad de las empresas privadas, se muestran nombres y direcciones ficticios.

#Poner varias ubicaciones como predeterminado 

Las ubicaciones que se mostraran son las sientes:
		Universidad Tecnológica de Puebla.
		Catedral de la Cd. de Puebla.
		Fuerte de Loreto.
		Fuente de los Frailes.

1.Primero se declara la funcion addDefaultMarkers en donde se almacenan todas las ubicaciones y se manda a llamar la funcion marker:

 function addDefaultMarkers() {
    var locations = [
        { coordinates: [ -98.1516198071536, 19.05847425], name: "utp" },
        { coordinates: [  -98.1983963875387, 19.04281015], name: "Catedral" },
        { coordinates: [-98.1870100851584, 19.0577841], name: "Fuertes" },
        { coordinates: [-98.2226237, 19.0537911], name: "Frailers" }
        // Add more locations as needed
    ];

    locations.forEach(function (location) {
        var marker = new atlas.Shape(new atlas.data.Point(location.coordinates));
        marker.addProperty('name', location.name);
        datasource.add([marker]);
    });
}


2.Despues se manda a llamar la funcion addDefaultMarkers dentro de la funcion GetMap: 

addDefaultMarkers();

3. Dentro de la variable map se pondra en el center las coordenadas de la Universidad Tecnologica de Puebla como punto de partida cundo el HTML se ejecute:

center: [-98.1516198071536, 19.05847425],
 
 Para agragar los botones con su correspondiente "acction" se utiliza el isguiente codigo HTML
 <body onload="GetMap()">
        <div id="myMap"></div>
        
        <div id="search">
            <div class="search-input-box">
                <div class="search-input-group">
                    <div class="search-icon" type="button"></div>
                    <input id="search-input" type="text" placeholder="Search">
                    <button onclick="selectStartPoint()">Seleccionar Punto de Inicio</button>
                    <button onclick="selectEndPoint()">Seleccionar Punto de Fin</button>
                    <button onclick="setCoordinatesFromInput()">Establecer Coordenadas</button>
                    <input type="text" id="coordinatesInput" placeholder="Coordenadas (latitud, longitud)">
        
                </div>
            </div>
            <ul id="results-panel"></ul>
        </div>
Se crean dos funciones para que el cliente indique el inicio de su ruta 
function selectStartPoint() {
    map.events.add('click', function (e) {
        var point = e.position;
        if (startPoint) {
            datasource.remove(startPoint);
        }
        startPoint = new atlas.data.Feature(new atlas.data.Point(point), {
            title: "Punto de Inicio",
            icon: "pin-blue"
        });
        datasource.add(startPoint);
        map.events.remove('click');
    });

Y otra para indicar su destino
function selectEndPoint() {
    map.events.add('click', function (e) {
        var point = e.position;
        if (endPoint) {
            datasource.remove(endPoint);
        }
        endPoint = new atlas.data.Feature(new atlas.data.Point(point), {
            title: "Punto de Fin",
            icon: "pin-round-blue"
        });
        datasource.add(endPoint);
        map.events.remove('click');
    });
}
Y se creo un evento para que el usuario por medio de click asignara las cordenadas y asu ves llamamos a la API de AZURE Maps
map.events.add('mousemove', showPopupWithInfo);

        var map, datasource, client;
        var startPoint, endPoint;

        function GetMap() {
            // Instantiate a map object
            map = new atlas.Map('myMap', {
                view: 'Auto',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: 'Uc70C9Tw0gxZyldQH5g1um2SPKBNEGVkk5G2Bez3pmE'
                    
                }
                
            });
Y se crean sus eventos para agregarlos al mapa con sus respectivos atributos

 map.events.add('ready', function () {
                // Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                // Create a layer for rendering the route lines and have it render under the map labels.
                map.layers.add(new atlas.layer.LineLayer(datasource, null, {
                    strokeColor: 'green',
                    strokeWidth: 5,
                    lineJoin: 'round',
                    lineCap: 'round'
                }), 'labels');

                // Create a layer for rendering point data.
                map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                    iconOptions: {
                        image: ['get', 'icon'],
                        allowOverlap: true
                    },
                    textOptions: {
                        textField: ['get', 'title'],
                        offset: [0, 1.2]
                    },
                    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
                }));

Se crea un GEOJSON para representar el inicio y el final del viaje

 startPoint = new atlas.data.Feature(new atlas.data.Point([-98.1516198071536, 19.05847425]), {
                    title: "Usted esta aqui",
                    icon: "pin-blue"
                });

                endPoint = new atlas.data.Feature(new atlas.data.Point([-98.2226237, 19.0537911]), {
                    title: "Destino",
                    icon: "pin-round-blue"
                });

Con ayuda de esta funcion se realiza el mapeo o enrutado

function calculateRoute() {
            if (startPoint && endPoint) {
                var pipeline = atlas.service.MapsURL.newPipeline(new atlas.service.MapControlCredential(map));
                var routeURL = new atlas.service.RouteURL(pipeline);
                var coordinates = [
                    [startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]],
                    [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]
                ];

                routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then(function (directions) {
                    var data = directions.geojson.getFeatures();
                    datasource.add(data);
                });
Se anexan evidencias:

[![Geolocalizacion-default.png](https://i.postimg.cc/GpjyC3Sq/Geolocalizacion-default.png)](https://postimg.cc/WDh3mVVk)
[![cordenados-por-usuario.png](https://i.postimg.cc/Tw4hdKjq/cordenados-por-usuario.png)](https://postimg.cc/xJL9gT4c)



