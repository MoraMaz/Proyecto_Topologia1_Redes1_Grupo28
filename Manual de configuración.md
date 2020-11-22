# Proyecto - Manual de configuración

[topol]: https://i.imgur.com/KiDrp6d.png "Topología"
[vtpd1]: https://i.imgur.com/hfT8qpQ.png "VTP en Distribucion1"
[vlad1]: https://i.imgur.com/ym26gWL.png "VLANs en Distribucion1"
[intd1]: https://i.imgur.com/3cnCVD1.png "Interfaces en Distribucion1"
[vtpd2]: https://i.imgur.com/uDzqyzX.png "VTP en Distribucion2"
[vlad2]: https://i.imgur.com/LPTHJD2.png "VLANs en Distribucion2"
[intr2]: https://i.imgur.com/fUzYHpT.png "Interfaces en R2"
[intn1]: https://i.imgur.com/ME1Gnuu.png "Interfaces en Nucleo1"
[intn2]: https://i.imgur.com/x7XZabh.png "Interfaces en Nucleo2"

## Topología 1

Antes de empezar a realizar la topología en GNS3, se debe establecer que nuestro número de grupo es 28, este se utilizará para cosas como generar las IPs que necesitan las redes del núcleo, obtener el número de cada una de las VLANs, etcétera. Es por esto que es necesario tener presente dicho número para generar las configuraciones correspondientes. Es por esto que la topología 1 que se solicita en el enunciado queda de la siguiente manera:

![Error al cargar la imagen :(][topol]


### Configuraciones

#### Distribución

La capa de distribución se compone por dos EtherSwitch llamados "Distribucion1" y "Distribucion2".

Antes de empezar a configurar lo solicitado, se deben configurar las interfaces de ambos EtherSwitch de la capa de distribución en modo troncal, esto se realiza con los siguientes comandos:

Para "Distribucion1":
```
configure terminal
    ip routing
    interface range f1/0 - 15
        speed 100
        duplex full
        switchport mode trunk
        no shutdown
        exit
    exit
```

Para "Distribucion2":
```
configure terminal
    no ip routing
    interface range f1/0 - 15
        speed 100
        duplex full
        switchport mode trunk
        no shutdown
        exit
    exit
```

Como se puede observar, lo único que cambia es el ruteo entre los EtherSwitch.

##### VTP

Para este caso, se debe configurar el EtherSwitch "Distribucion1" como servidor y para lograrlo, se deben ejecutar los siguientes comandos:

```
vlan database
    vtp domain t1grupo28
    vtp password t1grupo28
    vtp server
    vtp v2-mode
    exit
```

También se debe configurar el EtherSwitch "Distribucion2" como cliente, para lograr esto, se deben ejecutar los siguientes comandos:

```
vlan database
    vtp domain t1grupo28
    vtp password t1grupo28
    vtp v2-mode
    vtp client
    exit
```

Como se puede observar, lo único que cambia es el modo de configuración del protocolo (server/client) entre los EtherSwitch.

Ejecutando todos los anteriores comandos ya se tendría el protocolo configurado, lo que se puede comprobar con el comando:

```
show vtp status
```

Así se ven estas configuraciones:

![Error al cargar la imagen :(][vtpd1]

![Error al cargar la imagen :(][vtpd2]


##### VLANs

Como se había mencionado antes, los números de las VLANs dependen directamente del número de grupo, por lo que terminan siendo 38, 48, 58 y 68, con los nombres Red1, Red2, Red3 y Red4, respectivamente. 

Teniendo ya configurado el VTP del paso anterior, este protocolo debe replicar las VLANs del servidor a todos los clientes, en este caso, se deben configurar las VLANs en el EtherSwitch "Distribucion1" y por las configuraciones se deben replicar a "Distribucion2".

Para configurar las VLANs se ejecutan los siguientes comandos:

```
vlan database
    vlan 38 name RED1
    vlan 48 name RED2
    vlan 58 name RED3
    vlan 68 name RED4
    exit
```

Y en cualquiera de los dos EtherSwitch de Distribución, se puede comprobar estas con el comando:

```
show vlan-switch
```

Así se ven estas configuraciones:

![Error al cargar la imagen :(][vlad1]

![Error al cargar la imagen :(][vlad2]

Esto sería en el VTP y comprobando la replicación entre los dos EtherSwitch de la capa de Distribución, para terminar de configurar las VLANs, se necesita ejecutar los siguientes comandos en "Distribucion1":

```
configure terminal
    interface vlan 38
        description RED1
        ip address 192.168.10.1 255.255.255.0
        exit
    interface vlan 48
        description RED2
        ip address 192.168.20.1 255.255.255.0
        exit
    interface vlan 58
        description RED3
        ip address 192.168.30.1 255.255.255.0
        exit
    interface vlan 68
        description RED4
        ip address 192.168.40.1 255.255.255.0
        exit
    exit
```

Y para comprobar estas últimas configuraciones, se puede hacer con el comando:

```
sh ip interface brief
```

Así se ven estas configuraciones:

![Error al cargar la imagen :(][intd1]

##### Port-Channel

Se debe configurar Port-Channel entre los dos EtherSwitch de la capa de distribución, esto se realiza con los siguientes comandos (en ambos EtherSwitch):

```
configure terminal
    interface range f1/0 - 3
        channel-group 1 mode on
        exit
    exit
```

Al final se debe recordar que para guardar las configuraciones, en cada uno de los EtherSwitch se debe ejecutar el comando:

```
write memory
```

#### Acceso

La capa de acceso se compone por 4 switch y 4 host.

Las interfaces de los 4 switches se deben configurar en modo acceso en caso que conecten con los hosts y en modo truncal para las conexiones hacia los demás componetes (en este caso sólo es con la capa de distribución).

Los host son 4 máquinas, deberían de ser máquinas virtuales de Linux, pero por problemas de rendimiento en la PC, se utilizaron algunas VPCs para evitar la saturación del mismo.

##### Máquinas virtuales (Linux)

En este caso se utilizó VMware para crear las máquinas virtuales y poderlas utilizar en GNS3, para poder hacer esto, ya debe existir las máquinas en VMware. Estas son máquinas virtuales con sistema operativo TinyCore Linux por ahorro de recursos.

Como primer paso, para poder incluir la máquina virtual en la red de GNS3, se debe configurar un adaptador de red virtual en VMware.

Estas opciones se encuentran en Edit y luego Virtual Network Editor..., donde para crear un nuevo adaptador de red virtual se debe autorizar los cambios como administrador presionando en Change Settings y a este nuevo adaptador en Subnet IP se debe asignar la IP de la red según la topología, y se comprueba que la máscara de red sea la correcta.

Y este nuevo adaptador se debe asignar a la máquina virtual.

Al principio si se ejecuta el comando __*ifconfig*__ en una terminal en la máquina virtual no devolverá ninguna ip en concreto pues esta aún no ha sido configurada.

Para configurar la máquina virtual en la red, simplemente se abre ControlPanel, luego la sección Network y en IP Address se coloca la IP deseada, verificando que la máscara de red sea la correcta, se presiona enter y esto hará que se configure automáticamente el Broadcast y el Gateway de la máquina, se comprueba si estos datos son correctos y para guardar las configuraciones únicamente se presiona Apply y ya se puede cerrar esta sección.

Estas configuraciones ya se pueden comprobar con el comando __*ifconfig*__ en una terminal en el sistema.

Hasta este paso ya estará configurada la máquina virtual dentro de la red, justo como se describe en la topología.

Algo a tomar en cuenta es que sólo con esta configuración la máquina virtual solamente

##### VPCs

Para modificar las direcciones IP y la máscara en una Virtual PC se ejecuta el comando:

```
ip <direccion ip>/<máscara> <gateway>
```

Al final se debe recordar que para guardar las configuraciones, en cada VPC se debe ejecutar el comando:

```
save
```

#### Núcleo

El núcleo se compone por tres routers, llamados "R2", "Nucleo1" y "Nucleo2", la conexión de "R2" hacia los demás se realiza por conexión Serial, todas las demás conexiones se realizan por interfaces FastEthernet. El router "R2" será el que tendrá conexión hacia la VPN en la nube.

Las direcciones de red entre los Núcleos y las Distribuciones van totalmente relacionadas al número de grupo como se observa en la topología, estas son configuradas entre la capa de Núcleo y Distribución con los comandos que se muestran a continuación:

Para "R2":
```
configure terminal
    interface fastEthernet 0/0
        speed 100
        duplex full
        ip address 78.0.0.2 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 0/1
        speed 100
        duplex full
        ip address 88.0.0.2 255.255.255.0
        no shutdown
        exit
    exit
```

Para "Nucleo1":
```
configure terminal
    interface fastEthernet 0/0
        speed 100
        duplex full
        ip address 78.0.0.1 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 1/0
        speed 100
        duplex full
        ip address 58.0.0.1 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 0/1
        speed 100
        duplex full
        ip address 38.0.0.1 255.255.255.0
        no shutdown
        exit
    exit
```

Para "Nucleo2":
```
configure terminal
    interface fastEthernet 0/0
        speed 100
        duplex full
        ip address 88.0.0.1 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 1/0
        speed 100
        duplex full
        ip address 48.0.0.1 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 0/1
        speed 100
        duplex full
        ip address 68.0.0.1 255.255.255.0
        no shutdown
        exit
    exit
```

Para "Distribucion1":
```
configure terminal
    ip routing
    interface fastEthernet 0/0
        speed 100
        duplex full
        ip address 38.0.0.2 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 0/1
        speed 100
        duplex full
        ip address 48.0.0.2 255.255.255.0
        no shutdown
        exit
    exit
```

Para "Distribucion2":
```
configure terminal
    ip routing
    interface fastEthernet 0/0
        speed 100
        duplex full
        ip address 58.0.0.2 255.255.255.0
        no shutdown
        exit
    interface fastEthernet 0/1
        speed 100
        duplex full
        ip address 68.0.0.2 255.255.255.0
        no shutdown
        exit
    exit
```

##### RIP

Se debe configurar el Routing Information Protocol (RIP) en los núcleos, para poder configurar esto, se deben ejecutar los siguientes comandos:

Para "R2":
```
configure terminal
    router rip
        network 88.0.0.0
        network 78.0.0.0
        no auto-summary
        exit
    exit
```

Para "Nucleo1":
```
configure terminal
    router rip
        network 78.0.0.0
        network 58.0.0.0
        network 38.0.0.0
        no auto-summary
        exit
    exit
```

Para "Nucleo2":
```
configure terminal
    router rip
        network 88.0.0.0
        network 48.0.0.0
        network 68.0.0.0
        no auto-summary
        exit
    exit
```

Para "Distribucion1":
```
configure terminal
    router rip
        network 38.0.0.0
        network 48.0.0.0
        network 192.168.10.0
        network 192.168.20.0
        network 192.168.30.0
        network 192.168.40.0
        no auto-summary
        exit
    exit
```

Para "Distribucion2":
```
configure terminal
    router rip
        network 68.0.0.0
        network 58.0.0.0
        no auto-summary
        exit
    exit
```

Con estos comandos ya estaría configurado el protocolo.

Al verificar las interfaces de la capa de Núcleo, se distingue lo siguiente:

![Error al cargar la imagen :(][intr2]

![Error al cargar la imagen :(][intn1]

![Error al cargar la imagen :(][intn2]
