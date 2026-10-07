Instala o servidor BIND9 no equipo `darthvader`. Comproba que xa funciona coma servidor DNS caché pegando no documento de - entrega a saída deste comando `dig @localhost xunta.gal` 

![imaxe1](/imaxes/imaxe1.png)

Configura o servidor BIND9 no equipo mandalorian para que empregue como reenviador a darthvader pegando no documento de entrega contido do ficheiro /etc/bind/named.conf.options e a saída deste comando: `dig @localhost santiagodecompostela.gal.` Para un correcto funcionamento deberás borrar as root-hints do servidor mandalorian.

![imaxe2](/imaxes/imaxe2.png)

Instala unha zona primaria de resolución directa chamada "starwars.lan" e engade os seguintes rexistros de recursos (a maiores dos rexistros NS e SOA imprescindibles):

- Tipo A: darthvader con IP 192.168.20.10
- Tipo A: skywalker con IP 192.168.20.101
- Tipo A: skywalker con IP 192.168.20.111
- Tipo A: luke con IP 192.168.20.22
- Tipo A: darthsidious con IP 192.168.20.11
- Tipo A: yoda con IP 192.168.20.24 e 192.168.20.25
- Tipo A: c3p0 con IP 192.168.20.26
- Tipo CNAME palpatine a darthsidious
- TIPO MX con prioridade 10 sobre o equipo c3po
- TIPO TXT "lenda" con "Que a forza te acompanhe"
- TIPO NS con darthsidious

Pega no documento de entrega o contido do arquivo de zona, e do arquivo `/etc/bind/named.conf.local`

---

**named.conf.local**

```
//
// Do any local configuration here
//

zone "starwars.lan"{
    type primary;
    file "/etc/bind/db.starwars.lan";
};
zone "192.in-addr.arpa"{
    type primary;
    file "/etc/bind/db.20.168.192.in-addr.arpa";
};
```
---

**db.starwars.lan**

```
$TTL    86400
@       IN      SOA     darthvader.starwars.lan. miguel.starwars.lan. (
                        1   ;Número de serie
                        3600    ; Actualización (Refresh)
                        1800    ; Reintento (Retry)
                        1209600 ; Caducidade (Expire)
                        86400 ) ; TTL mínimo
                    
; Servidores de nomes

@       IN      NS      darthvader.starwars.lan.
@       IN      NS      darthsidious.starwars.lan.

; Rexistros A dos servidores de nomes

darthvader  IN  A   192.168.20.10
skywalker   IN  A   192.168.20.101
skywalker   IN  A   192.168.20.111
luke    IN  A   192.168.20.22
darthsidious    IN  A   192.168.20.11
yoda    IN  A   192.168.20.24
yoda    IN  A   192.168.20.25
c3p0    IN  A   192.168.20.26

; Alias (CNAME)

ftp     IN  CNAME   darthsidious

; Rexistro MX (servidor de correo)

@       IN      MX      10  c3p0.starwars.lan.
lenda   IN      TXT     "Que a forza de acompanhe"
```
---

Instala unha zona de resolución inversa que teña que ver co enderezo do equipo darthvader, e engade rexistros PTR para os rexistros tipo A do exercicio anterior. Pega no documento de entrega o contido do arquivo de zona, e do arquivo `/etc/bind/named.conf.local`

---

**db.192**

```
$TTL    86400
@       IN      SOA     darthvader.starwars.lan. miguel.starwars.lan. (
                        1   ;Número de serie
                        3600    ; Actualización (Refresh)
                        1800    ; Reintento (Retry)
                        1209600 ; Caducidade (Expire)
                        86400 ) ; TTL mínimo

; Servidores de nomes                        

@       IN      NS     darthvader.starwars.lan.
@       IN      NS     darthsidious.starwars.lan.

; Rexistros PTR

10      IN      PTR     darthvader.starwars.lan.
11      IN      PTR     darthsidious.starwars.lan.
22      IN      PTR     luke.starwars.lan.
24      IN      PTR     yoda.starwars.lan.
26      IN      PTR     c3p0.starwars.lan.
101     IN      PTR     skywalker.starwars.lan.
111     IN      PTR     skywalker.starwars.lan.
```
---

Comproba que podes resolver os distintos rexistros de recursos. Pega no documento de entrega a saída dos comandos:
- nslookup darthvader.starwars.lan localhost
- nslookup skywalker.starwars.lan localhost
- nslookup starwars.lan localhost
- nslookup -q=mx starwars.lan localhost
- nslookup -q=ns starwars.lan localhost
- nslookup -q=soa starwars.lan localhost
- nslookup -q=txt lenda.starwars.lan localhost
- nslookup 192.168.20.11 localhost


Pega nesta tarefa o enlace ao teu repo de github


