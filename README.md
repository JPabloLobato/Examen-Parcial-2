# Examen-Parcial-2
Curso: 4º cuatrimestre Ing.Informática | Redes
Github: https://github.com/JPabloLobato/Examen-Parcial-2.git

---

# Parte I – Misiones de Conocimiento Teórico (60%)
## Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes

| Departamento            | Subred              | Máscara           | Hosts útiles | Rango de hosts               | Broadcast         |
|------------------------|---------------------|-------------------|--------------|------------------------------|-------------------|
| Comando Central     | 172.16.0.0/26        | 255.255.255.192   | 62           | 172.16.0.1 - 172.16.0.62     | 172.16.0.63       |
| Defensa Perimetral | 172.16.0.64/27       | 255.255.255.224   | 30           | 172.16.0.65 - 172.16.0.94    | 172.16.0.95       |
| Centro Médico       | 172.16.0.96/27       | 255.255.255.224   | 30           | 172.16.0.97 - 172.16.0.126   | 172.16.0.127      |
| Hangar y Taller     | 172.16.0.128/28      | 255.255.255.240   | 14           | 172.16.0.129 - 172.16.0.142  | 172.16.0.143      |
| Enlace Troncal      | 172.16.0.144/28      | 255.255.255.240   | 14           | 172.16.0.145 - 172.16.0.158  | 172.16.0.159      |

Total de direcciones necesitadas 254 es decír 256.  
He elegido poner el enlace troncal en la última parte de la direccion ip ya que no sabemos cuantos hosts necesitaremos. 

### Elección de las máscaras
Para no desperdiciar ningún host me he tomado la molestia de utizar diferentes máscaras en la misma red.

## Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas

### ¿Que es el enrutamiento estático y dinámico
- **Enrutamiento estático:** se basa en la configuración manual de las rutas de los routers. El administrador de red debe configurar paso por paso cada ruta en la tabla de enrutamiento, esto conlleva un control total de la red.
- **Enrutamineto dinámico:** se apolla de protocolos que permiten a los routers intercambiar información sobre la topología de la red y calcular automáticamente las rutas más óptimas.  Como el algoritmo de Dijkstra que calcula la opción más rápida.

### Comparativa: Enrutamiento Estático vs Dinámico 

| Característica                | Enrutamiento Estático                                         | Enrutamiento Dinámico                                                  |
|------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------|
| Configuración             | Manual, requiere definir cada ruta                            | Automática mediante protocolos                                         |
| Actualización             | No se actualiza si cambia la topología                        | Se actualiza automáticamente ante cambios en la red                    |
| Administración            | Más simple en redes pequeñas                                  | Más compleja, requiere conocimiento de protocolos                      |
| Escalabilidad             | Limitada, hecha para redes pequeñas                          | Alta, ideal para redes grandes y con múltiples nodos                   |
| Tolerancia a fallos       | Baja, requiere reconfiguración manual                         | Alta, los protocolos redirijen el tráfico automáticamente            |
| Recursos necesarios       | Bajo uso de CPU y memoria                                     | Mayor uso de recursos (CPU, memoria, ancho de banda)                   |
| Seguridad                | Mayor control y seguridad al no divulgar información          | Menor seguridad si no se gestionan bien las actualizaciones dinámicas  |


---

### Protocolos de Enrutamiento Dinámico

#### Ejemplo: **RIP (Routing Information Protocol)**  
- Tipo: Vector de distancia  
- Métrica: Cantidad de saltos (máximo 15)  
- Ideal para: Redes pequeñas y simples  
- Limites: Convergencia lenta, propenso a bucles sin mecanismos adicionales (como *Split Horizon* o *Poison Reverse*)

#### Ejemplo: **OSPF (Open Shortest Path First)**  
- Tipo: Estado de enlace  
- Métrica: Costo basado en el ancho de banda  
- Ideal para: Redes grandes y complejas (como la HoloRed)  
- Ventajas: Convergencia rápida, conocimiento completo de la topología de red  
- Complejidad: Mayor configuración inicial, pero muy eficiente a gran escala

### Diferencias Clave: Vector de Distancia vs Estado de Enlace

| Elemento                      | Vector de Distancia (RIP)                 | Estado de Enlace (OSPF)                      |
|------------------------------|-------------------------------------------|----------------------------------------------|
| Conocimiento de red          | Solo conoce la distancia a destinos       | Conoce la topología completa                 |
| Actualización de rutas       | Periódica (cada 30 seg aprox)             | Al ocurrir cambios                           |
| Convergencia                 | Lenta                                     | Rápida                                       |
| Consumo de ancho de banda    | Bajo, pero frecuente                      | Bajo y eficiente                             |
| Escalabilidad                | Limitada                                  | Alta                                         |
| Complejidad                  | Baja                                      | Alta                                         |

#### Protocolos de vector de distancia
Calculan la mejor ruta hacia una red teniendo en cuenta la cantidad de saltos necesarios para llegar al destino. Cada router conoce únicamente la distancia y la dirección hacia la otra red. Tienen esta infomración porque la comparten entre sus vecinos.  
Son perfectos en redes pequeñas ya que son faciles de configurar, pero no son recomendables para redes grandes porque son lentos.

#### Protocolos de estado de enlace
Estos a diferencia, SOn más completa en cuanto a la topología de la red. Cada router intercambia información sobre sus enlaces directos con todos los demás routers de la red, lo que les permite construir una base de datos de toda la red y calculando así una mejor ruta mediante algoritmos como le que hemos esta utilizando el algoritmo de Dijkstra.

### Como puede afectar la caida repentina de un nodo
La caida de un nodo puede ocasionar diferentes probleams dependindo si es estático o dinámico.
Aquí algunos ejemplso:
- Interrupción del tráfico
- Rutas no válidas
- Retrasos al tener que hallar una nueva ruta
- Pérdida de datos
- Mayor carga en los nodos vecinos

## Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres
### ¿Qué es el DNS?
Su funcion principal es traducir nombre de dominios de la red en direcciones IP. 
Las máquinas no entienden que es por ejemplo `https://www.google.com` cosa que nosotros si, para ello esta el DNS que nos devuelve el ping correspondiente para llegar a la web https://www.google.com la cual por ejemplo podría ser `127.125.15.5`

### ¿Cómo funciona el DNS?
1. Consulta del cliente: nuestro dispositivo intenta acceder a `https://www.google.com`, el S.O (Sistema Operativo) pregunta al servidor DNS.
2. Resolución recursiva: si el servidor DNS no tiene la dirección IP, hace una búsqueda hata encontrar el servidor `google.com`.
3. Respuesta al cliente: cuando lo encuentra el servidor DNS devuelve al cliente la IP, permitiendo que se conecte.

### ¿Qué es un registro y que es el registro A?
Un registro DNS es una entrada en una base de datos asociada a los nombres de dominios.  
El registro A (Address): asocia un nombre de dominio con una dirección IPv4.

### ¿Qué sucede si el servidro DNS no está disponible?
Si el servidor DNS falla o se vuelve inestable generaría inaccesibilidad de servvicios haciendo que los dispositivos no puedan resolver nombres de dominio, siendo imposible acceder a los sitios web. Esto haría que para entrar a una web el usuario tenga que saber he introducir la dirección IP de la web.

---

## Misión 4: “Es una trampa… de protocolos!” – TCP vs UDP en las transmisiones

#### **Comparación de TCP y UDP**

| Característica                  | TCP (Protocolo de Control de Transmisión)          | UDP (Protocolo de Datagramas de Usuario)     |
|--------------------------------|----------------------------------------------------|---------------------------------------------|
| **Fiabilidad**                 | Altamente confiable: garantiza la entrega completa y ordenada. | No confiable: no garantiza entrega ni orden. |
| **Orientación**                | Orientado a conexión: establece una conexión antes de transmitir datos. | Sin conexión: no requiere establecer conexión previa. |
| **Manejo de errores**          | Detecta y corrige errores mediante retransmisión de paquetes perdidos. | No corrige errores ni retransmite paquetes. |
| **Velocidad**                  | Más lento debido al control y manejo de errores.   | Más rápido al eliminar controles y retransmisiones. |
| **Uso de recursos**            | Requiere más recursos para gestionar la conexión. | Menor uso de recursos: ideal para sistemas de baja latencia. |
| **Ejemplos de aplicaciones**   | Transferencia de archivos, correos electrónicos, navegación web. | Streaming de video/audio, juegos en línea, transmisiones en tiempo real. |


#### **Por qué TCP es confiable y orientado a conexión**
TCP es confiable y orientado a conexión ya que garantiza la entraga asegurandose que lleguen a su destino, manda los paquetes en orden y estable conxión antes de enviar cualquier dato estableciendo un canal de comunicación, al tener todo esto genera latencía y lentitud.

**Ejemplo galáctico**: Transmitir los planos de la Estrella de la Muerte a la base rebelde en un archivo comprimido para garantizar que toda la información llegue intacta y sin errores.

#### **Por qué UDP es no confiable y sin conexión**
La UDP es no confiable y sin conexión porque no asegura la llegada de los paquetes generando perdidas ocasionales y no envía los paquetes en orden, esto genera mayor rapider y baja latencia pero recordemos que puedes perder datos. Ideal para streaming.

**Ejemplo galáctico**: Transmitir coordenadas de combate en tiempo real a los pilotos X-Wing. Aunque se pierda alguna coordenada, la rapidez en la transmisión es crucial para evitar un ataque enemigo.

# Misión 5: Comunicación Segura o Lado Oscuro – Criptografía y Seguridad de la Red
