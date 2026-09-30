# Embús

Joc de trencaclosques de trànsit per al navegador. El tauler és un aparcament ple de vehicles, envoltat de carrers. Cada vehicle només pot avançar cap on apunta la seva fletxa: l'objectiu és treure'ls tots sense que xoquin entre ells, amb el trànsit del carrer ni saltant-se un semàfor en vermell.

Està fet amb HTML, CSS i JavaScript sense cap framework ni llibreria. Tot el joc és en un sol fitxer HTML.

## Fitxers

| Fitxer | Què és |
|---|---|
| `embus.html` | El joc complet: estructura, estils i codi. |
| `arrencada.mp3` | So d'arrencada del motor quan un vehicle surt. |
| `README.md` | Aquest document. |

Els dos primers han d'estar a la mateixa carpeta.

## Com obrir-lo

**Des del disc:** fes doble clic a `embus.html`. S'obrirà al navegador i ja es pot jugar.

**Amb un servidor local** (recomanat si el progrés no es desa quan l'obres des del disc; depèn del navegador):

```
python -m http.server 8000
```

Executa-ho dins de la carpeta del joc i obre `http://localhost:8000/embus.html`.

**En un servidor web:** puja `embus.html` i `arrencada.mp3` a la mateixa carpeta.

La tipografia (Overpass) es carrega de Google Fonts. Sense connexió a internet, el joc funciona igual amb una tipografia del sistema.

## Com es juga

- Toca (o fes clic a) un vehicle. Si té el camí lliure fins a la vora del tauler, surt al carrer, gira a la dreta i marxa.
- Si té un altre vehicle al davant, hi topa, torna a la seva plaça i perds una vida.
- Treu tots els vehicles per superar el nivell. Guanyes 10 monedes més 5 per cada vida que et quedi.
- Si et quedes sense vides, pots repetir el nivell (el tauler és el mateix) o continuar amb 1 vida pagant monedes.

### Trànsit (des del nivell 3)

- Pels carrers circulen cotxes grisos, blancs i negres, sense fletxa.
- Si un dels teus surt i xoca amb un cotxe del carril on s'incorpora (el més proper al tauler), torna a la seva plaça i perds una vida.
- El carril del sentit contrari no et fa perdre vides: aquells cotxes frenen per deixar-te girar.
- Si fas sortir dos vehicles pel mateix costat gairebé alhora, o si el vehicle ha d'entrar a una cruïlla ocupada, s'espera sol abans del carril i s'incorpora quan té lloc, sense perdre cap vida.
- Com més avances, més trànsit hi ha i més ràpid va.

### Semàfors (des del nivell 4)

- Alguns costats del tauler tenen semàfor, pas de vianants i línia de detenció. La línia s'il·lumina del color del semàfor.
- Si fas sortir un vehicle amb el llum vermell, frena, sona un xiulet i perds una vida. En verd i en groc pots passar.
- Mentre el llum és vermell, hi creuen vianants.
- Hi ha 1 costat amb semàfor al principi i en van apareixent més fins a arribar als 4.
- Els costats sense semàfor tenen la marca de cediu el pas.

### Ajudes

- **Pista:** marca en groc un vehicle que pot sortir. Si és possible, en tria un que tingui el semàfor en verd.
- **Bomba:** fa explotar el vehicle que triïs.
- **Reinicia:** torna a començar el nivell actual.
- **Des de zero** (bandera): torna al nivell 1 amb les monedes inicials. Demana confirmació.

### Teclat

| Tecla | Acció |
|---|---|
| Fletxes | Moure el cursor pel tauler |
| Espai o Retorn | Fer arrencar el vehicle del cursor |
| P | Pista |
| B | Bomba (Esc per cancel·lar-la) |
| R | Reiniciar el nivell |

## Nivells

Els nivells són infinits i es generen automàticament. Cada nivell sempre té solució. El mateix nivell, en la mateixa pantalla, genera sempre el mateix tauler.

La mida del tauler depèn del nivell i de la forma de la pantalla: en vertical és més alt i en horitzontal, més ample. Pot tenir entre 4 i 18 files i columnes, i mai queden caselles massa petites per tocar-les amb el dit. Si gires el mòbil a mig nivell, el tauler es reescala i s'adapta a la nova forma al nivell següent.

A partir del nivell 3 també hi apareixen camions, que ocupen 3 caselles.

## Configuració

Al principi del codi JavaScript de `embus.html` hi ha un bloc `var CONFIG={...}` (cerca `CONFIGURACIÓ DEL JOC`). Pots canviar-ne els valors, desar el fitxer i recarregar la pàgina (F5).

Els decimals s'escriuen amb punt (`1.5`), no amb coma, i cada línia ha d'acabar amb coma, excepte l'última.

### Vides

| Valor | Per defecte | Què fa |
|---|---|---|
| `VIDES` | `5` | Vides a cada nivell. Amb més de 6, el marcador mostra un cor amb el número. |

### Trànsit

| Valor | Per defecte | Què fa |
|---|---|---|
| `TRANSIT_DES_DEL_NIVELL` | `3` | Nivell on comença el trànsit. Amb `999` no n'hi ha mai. |
| `TRANSIT_SEPARACIO_INICIAL` | `5.2` | Segons entre cotxes de cada carril quan comença el trànsit. Més gran = menys cotxes. |
| `TRANSIT_SEPARACIO_PER_NIVELL` | `0.12` | Segons que es redueix la separació a cada nivell. Amb `0`, el trànsit no augmenta. |
| `TRANSIT_SEPARACIO_MINIMA` | `1.5` | Separació mínima: el màxim de trànsit que hi pot haver. |
| `TRANSIT_CARRIL_CONTRARI` | `1` | Multiplica la separació al carril contrari. Amb `2` hi ha la meitat de cotxes. |
| `TRANSIT_VELOCITAT` | `2.4` | Velocitat mínima dels cotxes, en caselles per segon. |
| `TRANSIT_VELOCITAT_VARIACIO` | `1.3` | Cada cotxe hi suma un valor a l'atzar entre 0 i aquest número. |
| `TRANSIT_VELOCITAT_PER_NIVELL` | `0.035` | Velocitat extra per cada nivell. |
| `TRANSIT_VELOCITAT_EXTRA_MAXIMA` | `1.4` | Límit de la velocitat extra per nivell. |
| `TRANSIT_CAMIONS` | `0.15` | Proporció de camions al trànsit (0 = cap, 1 = tots). |

Per exemple, per tenir la meitat de trànsit aproximadament: `TRANSIT_SEPARACIO_INICIAL:10` i `TRANSIT_SEPARACIO_MINIMA:3.5`.

### Semàfors

| Valor | Per defecte | Què fa |
|---|---|---|
| `SEMAFORS_DES_DEL_NIVELL` | `4` | Nivell on apareix el primer semàfor. N'hi ha 2 a partir de 4 nivells després, 3 a partir de 10 i 4 a partir de 18. |
| `SEMAFOR_VERD` | `5.5` | Segons en verd. |
| `SEMAFOR_GROC` | `1.5` | Segons en groc (encara es pot passar). |
| `SEMAFOR_VERMELL` | `4.5` | Segons en vermell. |

### Monedes

| Valor | Per defecte | Què fa |
|---|---|---|
| `MONEDES_INICIALS` | `60` | Monedes en començar a jugar o en començar des de zero. |
| `PREU_PISTA` | `20` | Preu d'una pista. |
| `PREU_BOMBA` | `40` | Preu d'una bomba. |
| `PREU_CONTINUAR` | `30` | Preu de continuar amb 1 vida quan les has perdut totes. |

### So

| Valor | Per defecte | Què fa |
|---|---|---|
| `SO_ARRENCADA` | `'arrencada.mp3'` | Fitxer d'àudio que sona quan surt un vehicle. |
| `VOLUM_ARRENCADA` | `0.8` | Volum, de 0 a 1. |

Els textos del joc (preus als botons, vides i nivells a "Com es juga") s'actualitzen sols amb aquests valors.

## So

El fitxer `arrencada.mp3` no és una gravació real: està generat per síntesi. Per posar-hi un so real, substitueix-lo per una gravació curta (1–2 segons) amb el mateix nom, o canvia `SO_ARRENCADA`. Assegura't que la llicència de la gravació en permeti l'ús.

Si el fitxer d'àudio falta o no es pot carregar, el joc fa servir un so d'arrencada sintetitzat pel navegador.

La resta de sons (xocs, xiulet del semàfor, explosió, victòria) també es generen amb el navegador i no necessiten cap fitxer.

El navegador no reprodueix cap so fins que no has tocat la pantalla una vegada. El botó de l'altaveu activa o desactiva el so.

## Progrés desat

El joc desa el nivell, les monedes i la preferència de so al navegador (`localStorage`, clau `embus-v1`). El progrés es manté entre sessions, però només en aquell navegador i dispositiu.

Per esborrar-lo, fes servir el botó **Des de zero**.

## Estructura del codi

Tot el codi JavaScript és a l'etiqueta `<script>` de `embus.html`, organitzat en seccions marcades amb comentaris:

| Secció | Contingut |
|---|---|
| Configuració | El bloc `CONFIG` descrit més amunt. |
| Generació de nivells | Col·loca els vehicles a l'atzar i comprova que el nivell té solució. |
| So | Reproducció de l'MP3 i sons sintetitzats. |
| Estat de la partida | Preparació de cada nivell i marcador. |
| Semàfors | Cicle de colors de cada semàfor. |
| Lògica | Moviment dels vehicles, xocs, pista, bomba, victòria i derrota. |
| Trànsit dels carrers | Carrils, cruïlles, seguiment entre cotxes i xocs amb el trànsit. |
| Dibuix | Ciutat, carrers, semàfors, vianants i vehicles (sobre un `<canvas>`). |
| Entrada | Ratolí, pantalla tàctil i teclat. |
