# Embús

Joc de trencaclosques de trànsit per al navegador. El tauler és un aparcament ple de vehicles, envoltat de carrers. Cada vehicle només pot avançar cap on apunta la seva fletxa. L'objectiu és treure'ls tots abans que s'acabi el temps, sense que xoquin entre ells ni amb el trànsit del carrer, i sense saltar-se cap semàfor en vermell.

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

## Dificultats

En començar una partida, el jugador tria la dificultat: **Principiant** (per defecte), **Mitjà**, **Alt** o **Superior**. Cada una té els seus propis valors de vides, temps, trànsit, semàfors, vehicles d'emergència i preus (vegeu [Configuració](#configuració)).

El selector apareix la primera vegada que s'obre el joc i quan es tria **Comença des de zero** al menú. Per canviar de dificultat cal començar des de zero.

## Com es juga

- Toca (o fes clic a) un vehicle. Si té el camí lliure fins a la vora del tauler, surt al carrer, gira a la dreta i marxa.
- Si té un altre vehicle al davant, hi topa, torna a la seva plaça i perds una vida.
- Treu tots els vehicles per superar el nivell. Guanyes 10 monedes més 5 per cada vida que et quedi.
- Si et quedes sense vides, pots repetir el nivell (el tauler és el mateix) o continuar amb 1 vida pagant monedes.

### Temps

Cada nivell té un compte enrere:

```
temps = TEMPS_FIX + (vehicles aparcats × TEMPS_PER_COTXE) + TEMPS_MARGE
```

`TEMPS_FIX` és comú a totes les dificultats; els altres dos valors depenen de la dificultat. Per exemple, a Principiant, un nivell amb 10 vehicles té 15 + 10 × 4 + 30 = 85 segons.

Els últims 10 segons, el rellotge es posa vermell i sona un avís cada segon. Si s'acaba el temps i encara queden vehicles:

- si tens prou monedes, pots comprar temps extra (per exemple, 30 segons per 10 monedes a Principiant);
- si no, has de tornar a començar el nivell.

### Trànsit

- Pels carrers circulen cotxes grisos, blancs i negres, sense fletxa. Com més avances, més trànsit hi ha i més ràpid va.
- Si un dels teus surt i xoca amb un cotxe del carril on s'incorpora (el més proper al tauler), torna a la seva plaça i perds una vida.
- El carril del sentit contrari no et fa perdre vides: aquells cotxes frenen per deixar-te girar.
- Si fas sortir dos vehicles pel mateix costat gairebé alhora, o si el vehicle ha d'entrar a una cruïlla ocupada, s'espera sol abans del carril i s'incorpora quan té lloc, sense perdre cap vida.

### Vehicles d'emergència

De tant en tant passen cotxes de policia, ambulàncies i camions de bombers, amb els llums intermitents encesos i una sirena curta quan apareixen. Si un dels teus hi xoca, perds 2 vides (configurable).

### Semàfors

- Alguns costats del tauler tenen semàfor, pas de vianants i línia de detenció. La línia s'il·lumina del color del semàfor.
- Si fas sortir un vehicle amb el llum vermell, frena, sona un xiulet i perds una vida. En verd i en groc pots passar.
- Mentre el llum és vermell, hi creuen vianants.
- Hi ha 1 costat amb semàfor al principi i en van apareixent més fins a arribar als 4.
- Els costats sense semàfor tenen la marca de cediu el pas.

### Botons

A dalt a la dreta hi ha dos botons:

- **Pausa:** atura el temps, el trànsit i els semàfors. El joc també es posa en pausa sol si canvies de pestanya.
- **Menú:** obre una finestra amb:
  - **Bomba:** fa explotar el vehicle que triïs. En activar-la, apareix un avís a dalt del tauler per cancel·lar-la.
  - **Reinicia el nivell.**
  - **So:** activa o desactiva el so.
  - **Com es juga.**
  - **Comença des de zero i tria la dificultat:** torna al nivell 1. Demana confirmació.

Mentre el menú o l'ajuda són oberts, el joc també queda aturat.

### Teclat

| Tecla | Acció |
|---|---|
| Fletxes | Moure el cursor pel tauler |
| Espai o Retorn | Fer arrencar el vehicle del cursor |
| P | Pausa (P o Esc per continuar) |
| M | Menú |
| B | Bomba (Esc per cancel·lar-la) |
| R | Reiniciar el nivell |

## Nivells

Els nivells són infinits i es generen automàticament. Cada nivell sempre té solució. El mateix nivell, en la mateixa pantalla, genera sempre el mateix tauler.

La mida del tauler depèn del nivell i de la forma de la pantalla: en vertical és més alt i en horitzontal, més ample. Pot tenir entre 4 i 18 files i columnes, i mai queden caselles massa petites per tocar-les amb el dit. Si gires el mòbil a mig nivell, el tauler es reescala i s'adapta a la nova forma al nivell següent.

A partir del nivell 3 també hi apareixen camions, que ocupen 3 caselles.

## Configuració

Al principi del codi JavaScript de `embus.html` hi ha un bloc `var CONFIG={...}` (cerca `CONFIGURACIÓ DEL JOC`). Pots canviar-ne els valors, desar el fitxer i recarregar la pàgina (F5).

- Els decimals s'escriuen amb punt (`1.5`), no amb coma.
- Cada línia ha d'acabar amb coma, excepte l'última de cada grup (just abans d'una `}`).

El bloc té dues parts: els valors comuns i una configuració per a cada dificultat, dins de `DIFICULTATS`.

### Valors comuns

| Valor | Per defecte | Què fa |
|---|---|---|
| `DIFICULTAT_PER_DEFECTE` | `'principiant'` | Dificultat marcada en començar una partida. Ha de ser una de les claus de `DIFICULTATS`. |
| `TEMPS_FIX` | `15` | Segons que s'afegeixen sempre al temps de cada nivell. |
| `SO_ARRENCADA` | `'arrencada.mp3'` | Fitxer d'àudio que sona quan surt un vehicle. |
| `VOLUM_ARRENCADA` | `0.8` | Volum de l'arrencada, de 0 a 1. |
| `SIRENES` | `true` | So de sirena quan apareix un vehicle d'emergència (`false` per treure'l). |

### Valors de cada dificultat

| Valor | Principiant | Mitjà | Alt | Superior | Què fa |
|---|---|---|---|---|---|
| `NOM` | Principiant | Mitjà | Alt | Superior | Nom que es mostra al joc. |
| `VIDES` | 5 | 4 | 3 | 2 | Vides a cada nivell. |
| `MONEDES_INICIALS` | 60 | 50 | 40 | 30 | Monedes en començar la partida. |
| `TEMPS_PER_COTXE` | 4 | 3 | 2.5 | 2 | Segons per cada vehicle aparcat. |
| `TEMPS_MARGE` | 30 | 20 | 12 | 6 | Segons de marge de la dificultat. |
| `TEMPS_EXTRA_SEGONS` | 30 | 30 | 20 | 15 | Segons que compres quan s'acaba el temps. |
| `TEMPS_EXTRA_PREU` | 10 | 15 | 15 | 20 | Monedes que costa el temps extra. |
| `PREU_BOMBA` | 40 | 45 | 50 | 60 | Preu de la bomba. |
| `PREU_CONTINUAR` | 30 | 40 | 50 | 60 | Preu de continuar amb 1 vida. |
| `TRANSIT_DES_DEL_NIVELL` | 3 | 2 | 1 | 1 | Nivell on comença el trànsit (`999` = mai). |
| `TRANSIT_SEPARACIO_INICIAL` | 6 | 5 | 4.2 | 3.5 | Segons entre cotxes de cada carril quan comença el trànsit. Més gran = menys cotxes. |
| `TRANSIT_SEPARACIO_PER_NIVELL` | 0.1 | 0.12 | 0.15 | 0.18 | Segons que es redueix la separació a cada nivell (`0` = no augmenta). |
| `TRANSIT_SEPARACIO_MINIMA` | 2.5 | 2 | 1.6 | 1.3 | Separació mínima: el màxim de trànsit. |
| `TRANSIT_CARRIL_CONTRARI` | 1.5 | 1.2 | 1 | 1 | Multiplica la separació al carril contrari (`2` = la meitat de cotxes). |
| `TRANSIT_VELOCITAT` | 2.2 | 2.4 | 2.8 | 3.2 | Velocitat mínima, en caselles per segon. |
| `TRANSIT_VELOCITAT_VARIACIO` | 1.0 | 1.3 | 1.4 | 1.5 | Cada cotxe hi suma un valor a l'atzar entre 0 i aquest. |
| `TRANSIT_VELOCITAT_PER_NIVELL` | 0.03 | 0.035 | 0.04 | 0.045 | Velocitat extra per cada nivell. |
| `TRANSIT_VELOCITAT_EXTRA_MAXIMA` | 1.0 | 1.4 | 1.6 | 1.8 | Límit de la velocitat extra. |
| `TRANSIT_CAMIONS` | 0.15 | 0.15 | 0.2 | 0.2 | Proporció de camions (0 = cap, 1 = tots). |
| `EMERGENCIA_PROBABILITAT` | 0.05 | 0.07 | 0.09 | 0.12 | Proporció del trànsit que són vehicles d'emergència (0 = cap). |
| `VIDES_XOC_EMERGENCIA` | 2 | 2 | 2 | 2 | Vides que perds si xoques amb un vehicle d'emergència. |
| `SEMAFORS_DES_DEL_NIVELL` | 4 | 3 | 2 | 1 | Nivell del primer semàfor. N'hi ha 2 a partir de 4 nivells després, 3 a partir de 10 i 4 a partir de 18. |
| `SEMAFOR_VERD` | 6 | 5.5 | 5 | 4.5 | Segons en verd. |
| `SEMAFOR_GROC` | 1.5 | 1.5 | 1.2 | 1 | Segons en groc (encara es pot passar). |
| `SEMAFOR_VERMELL` | 4 | 4.5 | 5 | 5.5 | Segons en vermell. |

Els textos del joc (preus, vides, temps extra, nivells a "Com es juga" i descripcions del selector de dificultat) s'actualitzen sols amb aquests valors.

**Afegir una dificultat nova:** copia un dels blocs de `DIFICULTATS` (per exemple, `mitja:{...}`), canvia-li la clau i el `NOM`, i posa una coma entre blocs. Apareixerà automàticament al selector.

## So

El fitxer `arrencada.mp3` no és una gravació real: està generat per síntesi. Per posar-hi un so real, substitueix-lo per una gravació curta (1–2 segons) amb el mateix nom, o canvia `SO_ARRENCADA`. Assegura't que la llicència de la gravació en permeti l'ús.

Si el fitxer d'àudio falta o no es pot carregar, el joc fa servir un so d'arrencada sintetitzat pel navegador.

La resta de sons (xocs, sirenes, xiulet del semàfor, avís dels últims segons, explosió, victòria) també es generen amb el navegador i no necessiten cap fitxer.

El navegador no reprodueix cap so fins que no has tocat la pantalla una vegada.

## Progrés desat

El joc desa la dificultat, el nivell, les monedes i la preferència de so al navegador (`localStorage`, clau `embus-v1`). El progrés es manté entre sessions, però només en aquell navegador i dispositiu.

Per esborrar-lo, fes servir **Comença des de zero** al menú.

## Estructura del codi

Tot el codi JavaScript és a l'etiqueta `<script>` de `embus.html`, organitzat en seccions marcades amb comentaris:

| Secció | Contingut |
|---|---|
| Configuració | El bloc `CONFIG` descrit més amunt. |
| Generació de nivells | Col·loca els vehicles a l'atzar i comprova que el nivell té solució. |
| So | Reproducció de l'MP3 i sons sintetitzats. |
| Estat de la partida | Preparació de cada nivell, marcador i temps. |
| Semàfors | Cicle de colors de cada semàfor. |
| Lògica | Moviment dels vehicles, xocs, bomba, victòria i derrota. |
| Trànsit dels carrers | Carrils, cruïlles, seguiment entre cotxes, vehicles d'emergència i xocs amb el trànsit. |
| Dibuix | Ciutat, carrers, semàfors, vianants i vehicles (sobre un `<canvas>`). |
| Entrada | Ratolí, pantalla tàctil i teclat; pausa i menú. |
| Dificultats | Selector de dificultat i aplicació dels seus valors. |

El joc fa servir un rellotge propi que només avança quan no està aturat (pausa, menú, ajuda o finestres de diàleg). Així el temps, el trànsit i els semàfors s'aturen tots alhora.
