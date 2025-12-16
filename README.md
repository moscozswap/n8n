Estructura para instalar un N8N limpio en un server.

1- Clonar este repositorio

2- Copiar el .env.example a .env y cambiar las credenciales

3- Crear la carpeta /vhost en /nginx y crear 2 ficheros: uno con el nombre del dominio y el otro igual pero con _location

En el primero poner:

if ($http_x_forwarded_proto = "https") {
    set $do_redirect "no";
}

En el _location:

proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";


Atencion:

Este ultimo paso se ha de hacer si el redireccionamiento a https se hace mediante Azure, y en caso de que no se haga tambien hay que quitar eesta linea en el docker de la app:

    - HTTPS_METHOD=noredirect