# Proyecto - Manual de comandos

## Capa de Distribución ~ VTP, VLANs y Port-Channel

Para "Distribucion1":
```
configure terminal                              # Entra en modo de configuración de la terminal
    ip routing                                  # Establece el modo de routeo en el EtherSwitch
    interface range f1/0 - 15                   # Selecciona un rango de interfaces del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        switchport mode trunk                   # Establece el modo troncal en las interfaces
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface range f1/0 - 3                    # Selecciona un rango de interfaces del dispositivo a configurar
        channel-group 1 mode on                 # Crea el Port-Channel en las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
vlan database                                   # Entra al modo de configuración de las vlans
    vtp domain t1grupo28                        # Crea el dominio del VTP con nombre t1grupo28
    vtp password t1grupo28                      # Establece la contraseña al VTP como t1grupo28
    vtp server                                  # Establece el modo servidor en el dispositivo
    vtp v2-mode                                 # Establece el modo v2
    vlan 38 name RED1                           # Crea la vlan 38 de nombre RED1
    vlan 48 name RED2                           # Crea la vlan 48 de nombre RED2
    vlan 58 name RED3                           # Crea la vlan 58 de nombre RED3
    vlan 68 name RED4                           # Crea la vlan 68 de nombre RED4
    exit                                        # Sale del modo de configuración de las vlans
show vtp status                                 # Muestra el estado del VTP
show vlan-switch                                # Muestra las vlans configuradas
configure terminal                              # Entra en modo de configuración de la terminal
    interface vlan 38                           # Selecciona una interfaz de vlan del dispositivo a configurar
        description RED1                        # Describe la vlan RED1
        ip address 192.168.10.1 255.255.255.0   # Establece la ip y la máscara de la interfaz
        exit                                    # Sale de la configuración de la interfaz
    interface vlan 48                           # Selecciona una interfaz de vlan del dispositivo a configurar
        description RED2                        # Describe la vlan RED2
        ip address 192.168.20.1 255.255.255.0   # Establece la ip y la máscara de la interfaz
        exit                                    # Sale de la configuración de la interfaz
    interface vlan 58                           # Selecciona una interfaz de vlan del dispositivo a configurar
        description RED3                        # Describe la vlan RED3
        ip address 192.168.30.1 255.255.255.0   # Establece la ip y la máscara de la interfaz
        exit                                    # Sale de la configuración de la interfaz
    interface vlan 68                           # Selecciona una interfaz de vlan del dispositivo a configurar
        description RED4                        # Describe la vlan RED4
        ip address 192.168.40.1 255.255.255.0   # Establece la ip y la máscara de la interfaz
        exit                                    # Sale de la configuración de la interfaz
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Distribucion2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    no ip routing                               # Elimina el modo de routeo en el EtherSwitch
    interface range f1/0 - 15                   # Selecciona un rango de interfaces del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        switchport mode trunk                   # Establece el modo troncal en las interfaces
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface range f1/0 - 3                    # Selecciona un rango de interfaces del dispositivo a configurar
        channel-group 1 mode on                 # Crea el Port-Channel en las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
vlan database                                   # Entra al modo de configuración de las vlans
    vtp domain t1grupo28                        # Crea el dominio del VTP con nombre t1grupo28
    vtp password t1grupo28                      # Establece la contraseña al VTP como t1grupo28
    vtp v2-mode                                 # Establece el modo v2
    vtp client                                  # Establece el modo cliente en el dispositivo
    exit                                        # Sale del modo de configuración de las vlans
show vtp status                                 # Muestra el estado del VTP
show vlan-switch                                # Muestra las vlans configuradas
write memory                                    # Guarda las configuraciones del dispositivo
```

## Capa de Acceso ~ IPs, Máscaras de red y Gateways

### VPCs

```
ip <direccion ip>/<máscara> <gateway>           # Establece la dirección IP, máscara y gateway a la VPC
save                                            # Guarda las configuraciones en la VPC
```

## Capa de Núcleo ~ Redes y RIP

Para "R2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    interface FastEthernet 0/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 78.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 0/1                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 88.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Nucleo1":
```
configure terminal                              # Entra en modo de configuración de la terminal
    interface FastEthernet 0/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 78.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 1/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 58.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 0/1                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 38.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Nucleo2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    interface FastEthernet 0/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 88.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface fastEthernet 1/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 48.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 0/1                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 68.0.0.1 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Distribucion1":
```
configure terminal                              # Entra en modo de configuración de la terminal
    ip routing                                  # Establece el modo de routeo en el EtherSwitch
    interface FastEthernet 0/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 38.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 0/1                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 48.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Distribucion2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    ip routing                                  # Establece el modo de routeo en el EtherSwitch
    interface FastEthernet 0/0                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 58.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    interface FastEthernet 0/1                  # Selecciona una interfaz del dispositivo a configurar
        speed 100                               # Establece la velocidad de las interfaces
        duplex full                             # Establece el modo de comunicación bidireccional simultánea
        ip address 68.0.0.2 255.255.255.0       # Establece la ip y la máscara de la interfaz
        no shutdown                             # Activa las interfaces
        exit                                    # Sale de la configuración de las interfaces
    exit                                        # Sale del modo de configuración de la terminal
show ip interface brief                         # Muestra las interfaces del dispositivo
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "R2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    router rip                                  # Entra en modo de configuración de RIP
        network 88.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 78.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        no auto-summary                         # Evita que RIP haga un resumen automático de la red
        exit                                    # Sale de la configuración de RIP
    exit                                        # Sale del modo de configuración de la terminal
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Nucleo1":
```
configure terminal                              # Entra en modo de configuración de la terminal
    router rip                                  # Entra en modo de configuración de RIP
        network 78.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 58.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 38.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        no auto-summary                         # Evita que RIP haga un resumen automático de la red
        exit                                    # Sale de la configuración de RIP
    exit                                        # Sale del modo de configuración de la terminal
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Nucleo2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    router rip                                  # Entra en modo de configuración de RIP
        network 88.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 48.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 68.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        no auto-summary                         # Evita que RIP haga un resumen automático de la red
        exit                                    # Sale de la configuración de RIP
    exit                                        # Sale del modo de configuración de la terminal
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Distribucion1":
```
configure terminal                              # Entra en modo de configuración de la terminal
    router rip                                  # Entra en modo de configuración de RIP
        network 38.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 48.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 192.168.10.0                    # Indica que dicha red sea notificada a los otros routers
        network 192.168.20.0                    # Indica que dicha red sea notificada a los otros routers
        network 192.168.30.0                    # Indica que dicha red sea notificada a los otros routers
        network 192.168.40.0                    # Indica que dicha red sea notificada a los otros routers
        no auto-summary                         # Evita que RIP haga un resumen automático de la red
        exit                                    # Sale de la configuración de RIP
    exit                                        # Sale del modo de configuración de la terminal
write memory                                    # Guarda las configuraciones del dispositivo
```

Para "Distribucion2":
```
configure terminal                              # Entra en modo de configuración de la terminal
    router rip                                  # Entra en modo de configuración de RIP
        network 68.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        network 58.0.0.0                        # Indica que dicha red sea notificada a los otros routers
        no auto-summary                         # Evita que RIP haga un resumen automático de la red
        exit                                    # Sale de la configuración de RIP
    exit                                        # Sale del modo de configuración de la terminal
write memory                                    # Guarda las configuraciones del dispositivo
```
