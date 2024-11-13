# Practica6

**Levantar os servizos**: 
Executa o seguinte comando no directorio onde se atopa o docker-compose.yml:
docker-compose up -d
**Acceder ao contedor cliente:**
Para comprobar que o cliente pode resolver nomes DNS, accede ao contedor:
docker exec -it cliente /bin/sh

**Comprobar a resolución DNS:** No interior do contedor, executa un comando como nslookup ou ping para verificar a resolución:
nslookup google.com
ping -c 4 google.com

Verificación:

Se a configuración está correcta, deberías ver que a resolución DNS é feita empregando o servidor asir_bind9 na IP 172.28.5.1. Se nslookup non está dispoñible na imaxe alpine, podes instalalo con:

apk update && apk add bind-tools

Isto asegurará que a función de cliente no contedor alpine está operativa e comunicándose co servizo DNS.
Aquí está o esquema de como implementar un docker-compose.yml para un servizo de DNS e un cliente nun contedor. Vou proporcionar o contido do arquivo docker-compose.yml, os pasos de instalación de paquetes e a configuración necesaria.
Explicación da configuración

    dns: Este servizo usa a imaxe bind9 como servidor DNS. Expón o porto 53 para permitir consultas DNS externas.
    client: Este servizo usa unha imaxe alpine, que mantén o contedor activo con tail -f /dev/null. Configúrase o DNS para que apunte ao servidor DNS interno (127.0.0.1).

Pasos de instalación de paquetes e comandos para probas

    Accede ao contedor do cliente:

docker exec -it dns-client sh

Instala bind-tools para usar dig:

apk update && apk add bind-tools

Proba o comando dig:

    dig @127.0.0.1 example.com

Comprobacións e Probas

    Asegúrate de que o ficheiro resolv.conf no contedor do cliente teña nameserver 127.0.0.1 configurado automaticamente grazas ao docker-compose.yml.
    Comproba que a consulta dig devolva unha resposta válida.

O repositorio incluirá:

    O ficheiro docker-compose.yml completo.
    Un ficheiro README.md explicando as instrucións de configuración, instalación de paquetes e probas realizadas.

README.md:

# Configuración de Servizo DNS con Cliente en Docker

## Estrutura do Proxecto
- `docker-compose.yml`: Contén a definición dos servizos DNS e cliente.
- Instalación de ferramentas necesarias para probas.

## Pasos de Instalación
Executa `docker-compose up -d` para levantar os contedores.

Accede ao contedor do cliente:

docker exec -it dns-client sh

Instala as ferramentas necesarias:

apk update && apk add bind-tools

Proba dig:

    dig @127.0.0.1 example.com

Explicación dos ficheiros .yml

dns: Servizo que actúa como servidor DNS.
client: Servizo cliente que usa o DNS configurado no docker-compose.yml.

Comandos de Proba

Proba de consulta DNS
dig @127.0.0.1 jacobo.com
Verificación de configuración de DNS
cat /etc/resolv.conf