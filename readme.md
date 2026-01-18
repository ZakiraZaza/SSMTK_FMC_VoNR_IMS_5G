# SSMTK_FMC_VoNR_IMS_5G
<table>
<tr>
  <th><b>Predmet</b></th>
  <td>Sistemi i servisi mobilnih telekomunikacija</td>
</tr>
<tr>
  <th><b>Akademska godina</b></th>
  <td>2025/2026</td>
</tr>
<tr>
  <th><b>Projektni zadatak</b></th>
  <td>Fiksno-mobilna konvergencija govorne usluge u 5G mreži</td>
</tr>
<tr>
  <th><b>Opis projektnog zadatka</b></th>
  <td>Dizajn i implementacija fiksno-mobilne konvergencije (FMC) koja povezuje VoNR u 5G mreži sa IMS/SIP govornom uslugom u fiksnoj mreži, kroz više scenarija IMS jezgra.</td>
</tr>
<tr>
  <th><b>Tim</b></th>
  <td align="left">
    <table>
      <tr>
        <th><b>Član tima</b></th>
        <th colspan="2"><b><a href="#radni-paketi">Alocirani radni paketi</a></b></th>
      </tr>
      <tr>
        <td>Zakira Jašarević</td>
        <td>RP1</td>
        <td>RP5</td>
      </tr>
      <tr>
        <td>Lejla Porobić</td>
        <td>RP1</td>
        <td>RP4</td>
      </tr>
      <tr>
      <td>
        Amr Saračević</td>
        <td>RP2</td>
        <td>RP3</td>
      </tr>
      <tr>
        <td>Emina Hasković</td>
        <td>RP3</td>
        <td>RP5</td>
      </tr>
      <tr>
        <td>Merjema Varupa</td>
        <td>RP4</td>
        <td>RP6</td>
      </tr>
      <tr>
        <td>Muhamed Crnčalo</td>
        <td>RP2</td>
        <td>RP5</td>
      </tr>
    </table>
  </td>
</tr>
</table>

# Teorijski uvod
Ovaj projekat se bavi dizajnom i praktičnom implementacijom fiksno-mobilne konvergencije (FMC) govorne usluge, gdje se VoNR (Voice over New Radio) u 5G mreži povezuje sa IMS/SIP baziranom govornom uslugom u fiksnoj mreži. Cilj je razumjeti i demonstrirati kako se govorni pozivi uspostavljaju kroz različite arhitekturne scenarije IMS jezgra, te kako se signalizacija i medijski tok ponašaju u realnom test okruženju.

## Definicija FMC-a i njegova važnost
Široka rasprostranjenost širokopojasnih bežičnih tehnologija otvorila je novo razdoblje konvergencije, u kojem se različiti bežični uređaji i pristupne tehnologije koriste za pristup velikom broju usluga. Ova konvergencija, poznata kao fiksno-mobilna konvergencija (FMC), podrazumijeva objedinjavanje bežičnih i žičnih govornih, video i širokopojasnih podatkovnih usluga kroz njihovu neometanu integraciju u jedinstvenu mrežnu cjelinu.[^1]. Dakle, FMC označava integraciju fiksnih i mobilnih servisa tako da korisnik dobije jedinstveno iskustvo, bez obzira da li pristupa kroz mobilnu (5G) ili fiksnu mrežu. U praksi, FMC se često realizuje kroz IMS (IP Multimedia Subsystem) kao servisni sloj za govor (i druge real-time usluge), uz SIP signalizaciju i RTP/RTCP.

## VoNR u 5G SA mreži – osnovni koncept
Voice over New Radio (VoNR) predstavlja proces enkapsulacije govora preko Internet protokola (VoIP) koristeći 5G radio-pristupnu i jezgrenu mrežnu arhitekturu. Time se omogućava prijenos medijskih komponenti preko novog radio-interfejsa, odnosno 5G mreže. Jednostavnije rečeno, VoNR obezbjeđuje govorne usluge u 5G samostalnim (standalone) mrežama.[^2]
I VoLTE i VoNR koriste IP Multimedia Subsystem (IMS), koji omogućava integraciju paketno baziranih poziva u mrežnu infrastrukturu, pri čemu je mobilna mreža izvor komunikacije. Razlika između VoLTE i VoNR leži u osnovnoj mobilnoj mreži i radio arhitekturi na kojoj su zasnovani. Voice over New Radio (VoNR) pretvara telefonske pozive u podatkovne pakete, koji se zatim prenose putem 5G mreže i IMS sistema.

Tipičan tok je:
- UE se registruje u 5GC (NAS/NGAP),
- uspostavlja se PDU sesija (user plane preko UPF),
- UE se zatim registruje na IMS (SIP REGISTER preko P-CSCF),
- poziv se uspostavlja SIP porukama (INVITE/180/200/ACK),
- govor ide preko RTP (medijski tok), uz QoS politike i prioritet govora.

## IMS i SIP – uloga u govornoj usluzi
IMS je arhitekturni okvir koji omogućava operaterima da pružaju govor kroz IP (multimedijalne) servise. Ključne IMS komponente (konceptualno) uključuju:
- P-CSCF (prva SIP tačka za UE, sigurnost i politika),
- I-CSCF / S-CSCF (routing, registracija i servisna logika),
- baza korisnika (HSS/UDM u modernim implementacijama),
- interkonekcijski elementi (npr. SBC/IBCF) za povezivanje sa drugim mrežama,
- aplikacijski serveri (npr. call services), te medijski gateway elementi kada je potrebno.

## Scenariji koji se analiziraju
U projektu se razmatraju tri realna scenarija organizacije IMS jezgra:
- Zajedničko IMS jezgro u 5G mreži: IMS je “u mobilnoj domeni”, a fiksna mreža se veže kao SIP/IMS interkonekcija ili trunk prema tom IMS-u.
- Zajedničko IMS jezgro u fiksnoj mreži: 5G mreža koristi zajedničko IMS jezgro koje je smješteno u fiksnoj domeni.
- Odvojena IMS jezgra (5G IMS i fiksni IMS): Mobilna i fiksna mreža imaju svoje IMS sisteme, koji se povezuju preko SIP peering / interkonekcije (često kroz SBC/IBCF), uz definisana pravila routinga i autentifikacije.
U praktičnom dijelu projekta fokus je na implementaciji i mjerenjima za scenarije (1) i (3), uz poređenje signalizacije i ponašanja sistema.

# RP1: Dizajn koncepta fiksno-mobilne konvergencije (FMC)
U okviru RP1 modelirana su tri arhitekturna scenarija organizacije IMS jezgra u kontekstu FMC govorne usluge.  
U svim scenarijima razlikuju se pristupni domeni (5G mobilna mreža i fiksna pristupna mreža), dok se govor realizuje preko IMS/SIP sloja. Razlika je u tome **gdje se IMS jezgro fizički/logički nalazi** i kako su međusobno povezani mobilni i fiksni korisnici.

---

### Zajednički elementi arhitekture
#### 5G mobilna mreža
- **5G UE (VoNR telefon)** – 5G pametni telefon koji podržava VoNR i sadrži IMS parametre (APN za IMS, SIP domen).  
- **gNB** – 5G bazna stanica koja obezbjeđuje radio pristup i prosljeđuje saobraćaj prema 5G Core-u.  
- **5G Core (AMF/SMF/UPF)**  
  - **AMF** – upravlja pristupom i mobilnošću UE-a (NAS signalizacija, registracija).  
  - **SMF** – upravlja PDU sesijama, IP adresama i QoS parametrima.  
  - **UPF** – provodi korisnički saobraćaj (SIP/RTP) između 5G RAN-a i servisnih platformi (IMS, Internet itd.).

#### Fiksna mreža
- **SIP telefon / Softphone** – krajnji uređaj fiksnog korisnika (IP telefon ili softphone).  
- **CPE / Home router** – kućni ruter koji pruža lokalnu IP konekciju prema operatoru (NAT, osnovni QoS).  
- **Fiksna pristupna tačka (xDSL/FTTH)** – pristupni segment koji povezuje CPE sa mrežom operatora.  
- **AGF / BNG (Fixed Access Gateway)** – agregacioni čvor koji terminiše sesije fiksnih korisnika, obavlja IP agregaciju i QoS te usmjerava SIP/RTP saobraćaj prema IMS-u.

#### IMS jezgro (logičke funkcije)
U zavisnosti od scenarija, IMS jezgro je smješteno u 5G ili u fiksnoj mreži, ili postoje dva odvojena IMS domena. U svim slučajevima IMS obuhvata:
- **P-CSCF** – prvi kontakt za SIP signalizaciju iz mreže.  
- **I-CSCF** – ulazna tačka IMS domena, bira odgovarajući S-CSCF.  
- **S-CSCF** – centralni SIP server koji održava registracije i sesije korisnika te primjenjuje servisna pravila.  
- **AS (Application Servers)** – aplikacijski serveri za dodatne usluge (FMC logika, istovremeno zvonjenje, voicemail itd.).  
- **HSS/UDM** – pretplatnička baza sa profilima mobilnih i fiksnih korisnika (brojevi, identiteti, servisi).

---

### Scenarij (1): Zajedničko IMS jezgro u 5G mreži
U prvom scenariju IMS jezgro je smješteno u **mobilnoj (5G) domeni** i koristi se kao zajednička platforma za VoNR i fiksnu govornu uslugu.

**Tok poziva (primjer 5G UE → fiksni SIP korisnik):**
- 5G UE se registruje u 5GC i uspostavlja PDU sesiju prema IMS APN-u, zatim se registruje na IMS (SIP `REGISTER` preko P-CSCF-a).  
- Fiksni SIP telefon se, preko CPE-a i AGF/BNG-a, registruje na isto IMS jezgro.  
- Pri uspostavi poziva, SIP `INVITE` sa 5G UE prolazi kroz 5GC do IMS-a, gdje S-CSCF pronalazi registraciju fiksnog korisnika i prosljeđuje poziv prema fiksnoj mreži.  
- RTP tok nakon uspostave sesije prati put:
  - `5G UE -> gNB -> UPF -> IP jezgro -> AGF/BNG -> CPE -> SIP telefon`.

Ovaj scenarij naglašava FMC u kojem je **5G mreža “domicilna” za IMS**, a fiksna mreža ulazi kao dodatni pristupni domen.

<div align="center">
  <img src="assets/diagrams/draw_io/images/common_ims_core_in_5g_network.svg" alt="common_ims_core_in_5g_network.svg" title="Zajedničko IMS jezgro u 5G mreži">
  <br>
  <i>Slika 1: Zajedničko IMS jezgro u 5G mreži</i>
</div>

---

### Scenarij (2): Zajedničko IMS jezgro u fiksnoj mreži
U drugom scenariju IMS jezgro je i dalje zajedničko za mobilne i fiksne korisnike, ali je **smješteno u fiksnoj mreži**. 5G domen koristi IMS fiksnog operatora za pružanje VoNR usluge.

**Tok poziva (5G UE → fiksni SIP korisnik):**
- 5G UE se registruje u 5GC, zatim šalje SIP `REGISTER` ka IMS-u u fiksnoj mreži (tunelovan preko UPF-a).  
- Fiksni SIP telefon se registruje lokalno na isto IMS jezgro u fiksnoj mreži.  
- SIP `INVITE` sa 5G UE ide: `UE → gNB → 5GC → IMS P-CSCF (u fiksnoj mreži) → S-CSCF`, koji zatim poziv prosljeđuje prema registraciji fiksnog korisnika.  
- Media saobraćaj teče preko UPF-a i IP jezgra prema fiksnoj strani.

Ovaj scenarij odgovara situaciji u kojoj **fiksni IMS postoji kao centralna platforma**, a 5G mreža ga koristi kao servisni sloj za govor.

<div align="center">
  <img src="assets/diagrams/draw_io/images/common_ims_core_in_a_fixed_network.svg"  alt="common_ims_core_in_a_fixed_network.svg" title="Zajedničko IMS jezgro u fiksnoj mreži">
  <br>
  <i>Slika 2: Zajedničko IMS jezgro u fiksnoj mreži</i>
</div>

---

### Scenarij (3): Odvojena IMS jezgra za 5G i fiksnu mrežu
Treći scenarij predstavlja pristup u kojem 5G i fiksna mreža imaju **dva odvojena IMS domena**, sa zasebnim pretplatničkim bazama i servisnim logikama. Između ova dva IMS sistema uspostavljen je SIP trunk / peering.

**Tok poziva (5G UE → fiksni SIP korisnik):**

- 5G UE je registrovan na IMS-5G, a fiksni SIP telefon na IMS-F; svaki koristi vlastitu pretplatničku bazu i servisni profil.  
- SIP `INVITE` sa 5G UE ide prema S-CSCF-5G, koji na osnovu broja ili domene prepoznaje da se pozvani korisnik nalazi u fiksnoj IMS mreži.  
- Poziv se preusmjerava preko SIP trunk veze: `IMS-5G → IMS-F`, gdje S-CSCF-F pronalazi registraciju fiksnog korisnika i dostavlja poziv do SIP telefona.  
- Media saobraćaj se uspostavlja direktno između domena ili preko media gateway-a, u zavisnosti od konfiguracije trunk-a i eventualnog transkodiranja.

U ovom scenariju konvergencija se ostvaruje **na nivou interkonekcije dva IMS sistema**, a ne kroz jedno zajedničko jezgro, što omogućava veću nezavisnost domena, ali i kompleksnije upravljanje routiranjem i politikama.

<div align="center">
  <img src="assets/diagrams/draw_io/images/separate_ims_cores_for_5G_and_fixed_network.svg" alt="separate_ims_cores_for_5G_and_fixed_network.svg" title="Odvojena IMS jezgra za 5G i fiksnu mrežu">
  <br>
  <i>Slika 3: Odvojena IMS jezgra za 5G i fiksnu mrežu</i>
</div>

---

# RP2: Implementacija VoNR usluge korištenjem AMARI Callbox Mini rješenja i 5G mobilnih telefona
U okviru eksperimenta uspješno je izvršeno povezivanje korisničkog uređaja na 5G mrežu i verifikovana osnovna funkcionalnost mrežnog i servisnog sloja.

### Uspostava 5G 
Prelazak na 5G vrši se pozivom sljedećih naredbi kao root unutar foldera /enb:

```shell
ln -sfn gnb-sa.cfg enb.cfg
service lte start
```

Mobilni uređaj je uspješno registrovan na baznu stanicu i ostvarena je 5G (NR) konekcija, što je potvrđeno statusom mreže na uređaju. Tokom testiranja, uređaj je radio u 5G režimu sa stabilnim radio linkom.

### Verifikacija podatkovne konekcije
Izvršeno je mjerenje performansi mreže. Ostvarene su stabilne vrijednosti download i upload brzine, uz prihvatljive vrijednosti kašnjenja i jittera, bez detektovanog gubitka paketa. Ovi rezultati potvrđuju ispravnu uspostavu podatkovnog prenosa preko 5G mreže.

<div align="center">
  <img src="assets/5g/images/connection_verification.png" alt="connection_verification.png" title="Verifikacija podatkovne konekcije" style="width:50%">
  <br>
  <i>Slika 4: Verifikacija podatkovne konekcije</i>
</div>

---

### Uspostava poziva
Nakon uspješne registracije na mrežu, na mobilnom uređaju je uspostavljen poziv čime je potvrđena ispravna signalizacija i funkcionalnost servisnog sloja. Poziv je izvršen sljedećom komandom:

```
mt_call 0600000124
```

Poziv je iniciran i održan bez prekida, što ukazuje na pravilno funkcionisanje mrežne infrastrukture i povezanih servisa.

<div align="center">
  <img src="assets/5g/images/call_setup.jpg" alt="call_setup.jpg" title="Uspostava poziva" style="width:30%">
  <br>
  <i>Slika 5: Uspostava poziva</i>
</div>

---

### Snimanje mrežnog saobraćaja upotrebom Wireshark alata za VoNR u 5G mreži
Tokom testiranja uspostave VoNR poziva izvršeno je snimanje mrežnog saobraćaja u .pcap formatu radi kasnije analize signalizacijskih i transportnih tokova. Snimanje je realizovano korištenjem alata `tcpdump`, na relevantnom mrežnom interfejsu AMARI sistema, u realnom vremenu. 

Dobijeni .pcap fajl sadrži:
- SCTP saobraćaj (NGAP signalizacija između gNB i 5GC),
- GTP tunelovani saobraćaj (korisnički i kontrolni tokovi),
- heartbeat poruke koje potvrđuju aktivne i stabilne veze između mrežnih elemenata.
  
Signalizacijski i govorni tokovi u VoNR scenariju su kriptovani:
- IMS signalizacija koristi SIP over TLS,
- govorni tok koristi SRTP.
Zbog toga sadržaj SIP poruka i audio signala nije direktno vidljiv u Wiresharku, dok su dostupni meta-podaci (protokoli, vremenski odnosi, redoslijed paketa), što odgovara realnim operativnim 5G mrežama.

<div align="center">
  <img src="assets/5g/images/wireshark_rp2.png" alt="wireshark_rp2.png" title="Snimanje mrežnog saobraćaja u Wireshark alatu za VoNR u 5G mreži" style="width:60%">
  <br>
  <i>Slika 6: Snimanje mrežnog saobraćaja u Wireshark alatu za VoNR u 5G mreži</i>
</div>

---

### SIP klijent – `pjsua` konfiguracijska datoteka
Konfiguracijska datoteka `pjsua.cfg` korištena je za:
  - registraciju SIP korisnika na IMS jezgro 5G mreže,
  - verifikaciju ispravnosti IMS/SIP signalizacije,
  - testiranje dostupnosti govorne usluge prije realizacije FMC scenarija.

Ovaj korak je neophodan kako bi se potvrdila funkcionalnost IMS sloja nezavisno od mobilnog korisničkog uređaja.

`pjsua` je odabran jer omogućava potpunu kontrolu SIP parametara kroz konfiguracijsku datoteku, daje jasan uvid u proces SIP registracije i odgovore IMS jezgra, te je pogodan za eksperimentalnu i akademsku analizu SIP/IMS sistema.

Datoteka `pjsua.cfg` kreirana je sa ciljem definisanja SIP identiteta korisnika (IMPU/IMPI), IMS registra, autentifikacijskih parametara, lokalnog SIP porta i ponašanja prilikom dolaznih poziva.

```bash
--realm *
--registrar sip:192.168.4.1
--id sip:1234
--username sipclient
--password sipclient
--local-port 5061
--auto-answer=200
```

Na osnovu ove konfiguracije ostvarena je uspješna SIP registracija korisnika `(200 OK)`, aktivan SIP nalog sa statusom Online, periodično IMS re-registriranje, spremnost sistema za uspostavu govorne sesije. Ovim je potvrđena ispravna konfiguracija SIP klijenta i IMS jezgra, čime je obezbijeđena osnova za dalju realizaciju fiksno-mobilne konvergencije u narednim radnim paketima.

<div align="center">
  <img src="assets/5g/images/pjsua.png" alt="pjsua.png" title="Korištenje <code>pjsua.cfg</code> konfiguracijske datoteke za SIP registraciju korisnika" style="width:60%">
  <br>
  <i>Slika 7: Korištenje <code>pjsua.cfg</code> konfiguracijske datoteke za SIP registraciju korisnika</i>
</div>

---

# RP3: Implementacija FMC za scenarij (1)
U okviru scenarija (1) pokušana je realizacija fiksno-mobilne konvergencije, tako što će se fiksni korisnik (SIP softphone) registrovati na isti IMS koji koristi VoNR u 5G mreži (Amarisoft Callbox Mini). Zbog nedostaka fizičkog SIP telefona, kao fiksni terminal korišten je `Linphone` (desktop). Definisan je SIP korisnika u IMS bazi (`ue_db-ims.cfg / edb.cfg`)

Kreiran IMS korisnik za SIP klijenta koristeći standardni IMS pristup:
- `IMPI`: sipclient (privatni identitet za autentifikaciju)
- `IMPU`: 1234 i tel:1234 (javni identiteti za pozivanje)
- `pwd`: sipclient (lozinka)
- `authent_type`: MD5 (tip autentifikacije)
- `domain`: ims.mnc001.mcc001.3gppnetwork.org (domena)

Zatim je realizovana registracija Linphone-a na IMS (Digest/MD5).

Linphone je podešen da se registruje na IMS kroz:
- `Identity` tipa sip:sipclient@...
- `Proxy/Server` prema IP adresi Callbox Mini (SIP port 5060)
- pokušaj usklađivanja Realm/UserID/Password parametara za MD5 autentifikaciju

U više pokušaja rađeno je otklanjanje grešaka (debugging) kroz iterativno usklađivanje parametara i mijenjano je sljedeće:
- `Realm` (IP vs IMS domena), 
- `User ID` (IMPI vs prazno) i 
- `Format identity` (sipclient@IP vs sipclient@IMS_domain). 

Linphone nije uspio završiti IMS registraciju. Indikator greške koji je ostao je (uzvičnik) i poruka: “Unable to authenticate. Please verify your password.”, što znači da autentifikacija nije uspjela, te da je potrebno verifikovati lozinku. 

Zbog neuspješne registracije SIP klijenta, FMC poziv (Linphone - VoNR UE) nije mogao biti uspostavljen u ovom koraku. Screenshot prikazuje definisanog SIP korisnika i istovremeno status neuspjele registracije u Linphone-u, što potvrđuje da problem nastaje u fazi IMS Digest autentifikacije.

<div align="center">
  <img src="assets/5g/images/linphone.jpg" alt="linphone.jpg" title="Pokušaj registracije Linphone SIP klijenta na IMS (zajedničko IMS jezgro u 5G mreži)" style="width:60%">
  <br>
  <i>Slika 8: Pokušaj registracije Linphone SIP klijenta na IMS (zajedničko IMS jezgro u 5G mreži)</i>
</div>

---

> [!NOTE]
> Mogući razlog neuspjeha jeste neusklađen Realm/Domen u Digest autentifikaciji.  Linphone je u pojedinim pokušajima koristio Realm kao IP adresu, dok IMS očekuje domen ims.mnc001.mcc001.3gppnetwork.org, što može dovesti do pogrešnog MD5 hash-a i odbijanja registracije. Također, moguće su kontradiktorne odnosno duple postavke u `edb.cfg` konfiguracijskog datoteci. U konfiguraciji se mogu vidjeti elementi za “standard SIP client” i istovremeno definisan SIP user sa MD5 autentifikacijom, što može uzrokovati konflikte u pravilima autentifikacije.

U narednom koraku je planirano očistiti IMS bazu i ostaviti samo jedan SIP korisnički blok (bez duplikata i bez `authent_type: none`) i zatim ponovo pokrenuti servis (`lte_stop / lte_start`). Zatim, potrebno je utvrditi tačan Realm koji IMS šalje u 401 izazovu iz `lteims.log`, te u Linphone-u eksplicitno postaviti parametre u skladu s tim.

---

# RP4 – Implementacija FMC za scenarij (3): odvojena IMS jezgra 5G i fiksne mreže
U okviru RP4 pokušana je realizacija fiksno-mobilne konvergencija (FMC) u scenariju u kojem 5G mreža i fiksna SIP mreža imaju odvojena IMS jezgra. Integracija je ostvarena korištenjem Asterisk-a kao SIP gateway-a između fiksne SIP domene i IMS jezgra 5G mreže (Amarisoft Callbox Mini).

## Arhitektura rješenja (RP4 kontekst)
U ovom scenariju:
- 5G korisnici koriste VoNR/VoLTE preko IMS jezgra u 5G mreži,
- fiksni korisnici koriste SIP softphone (Linphone),
- Asterisk posreduje signalizaciju i RTP tokove između dvije domene,
- IMS jezgra nisu zajednička, već međusobno povezana preko SIP trunk-a.

## SIP klijent – Linphone konfiguracija (fiksna mreža)
Na slici je prikazana konfiguracija Linphone SIP klijenta koji se registruje na Asterisk server kao fiksni korisnik (ekstenzija `6001`).  
Konfigurisani su osnovni SIP parametri:
- SIP URI (`sip:6001@<IP_ASTERISK>`),
- SIP server (Asterisk, UDP transport),
- period registracije i transportni protokol.

<div align="center">
  <img src="assets/5g/images/linphone-sip_client_configuration.png" alt="linphone-sip_client_configuration.png" title="Linphone - Konfiguracija SIP klijenta" style="width:100%">
  <br>
  <i>Slika 9: Linphone - Konfiguracija SIP klijenta</i>
</div>

---

## Asterisk – PJSIP konfiguracija (IMS SIP trunk)
U fajlu `/etc/asterisk/pjsip.conf` definisan je:
- IMS SIP trunk (`ims-endpoint`) koji povezuje Asterisk sa IMS jezgrom 5G mreže,
- dozvoljeni audio kodeci (`ulaw`, `alaw`) radi kompatibilnosti sa SIP/IMS interworking-om,
- SIP identifikacija (`identify`) prema IP adresi IMS-a,
- lokalni SIP endpoint za fiksnog korisnika (`6001`) sa autentifikacijom.

Ova konfiguracija omogućava ispravno uspostavljanje SIP sesija između Asterisk-a i IMS-a.

<div align="center">
  <img src="assets/5g/images/pjsip_conf-1.png" alt="pjsip_conf-1.png" title="Prikaz pjsip.conf konfiguracijske datoteke - 1" style="width:75%">
  <br>
  <i>Slika 10: Prikaz <code>pjsip.conf</code> konfiguracijske datoteke - 1</i>
</div>

---

<div align="center">
  <img src="assets/5g/images/pjsip_conf-2.png" alt="pjsip_conf-2.png" title="Prikaz pjsip.conf konfiguracijske datoteke - 2" style="width:75%">
  <br>
  <i>Slika 11: Prikaz <code>pjsip.conf</code> konfiguracijske datoteke - 2</i>
</div>

---

<div align="center">
  <img src="assets/5g/images/extensions_conf.png" alt="extensions_conf.png" title=" Prikaz extensions.conf konfiguracijske datoteke" style="width:75%">
  <br>
  <i>Slika 12: Prikaz <code>extensions.conf</code> konfiguracijske datoteke</i>
</div>

---

## Asterisk – rutiranje poziva
U fajlu **`/etc/asterisk/extensions.conf`** definisana su pravila rutiranja poziva između fiksne SIP domene i IMS domene:

- **`[internal]`**  
  - lokalni pozivi između SIP ekstenzija,
  - rutiranje odlaznih poziva iz fiksne mreže ka IMS-u (`Dial(PJSIP/ims-endpoint/${EXTEN})`).

- **`[from-ims]`**  
  - dolazni pozivi iz IMS/5G mreže prema fiksnom SIP korisniku (`6001`).

Dodani su `NoOp()` zapisi radi lakšeg praćenja signalizacijskog toka u Asterisk CLI-u.

## MicroSIP SIP klijent

U okviru projekta testiran je i MicroSIP (v3.22.3) kao alternativni SIP softphone u kontekstu FMC testiranja, s ciljem provjere da li se fiksni SIP klijent može direktno registrovati na Amarisoft IMS i koristiti za uspostavu poziva.
U testu su korišteni sljedeći parametri iz postojeće IMS konfiguracije i baze korisnika:

- IMS domena: ims.mnc001.mcc001.3gppnetwork.org
- IMS server: Amarisoft-IMS-2020-09-14
- SIP korisnik (IMPU): 1234@ims.mnc001.mcc001.3gppnetwork.org (definisan u ue_db-ims.cfg)
- Autentikacija: SIP Digest (MD5), korisnički identitet sipclient
- Transport: UDP (MicroSIP default)
- IMS IP adresa (registrar): 192.168.200.160 (REGISTER prema sip:192.168.200.160)

MicroSIP je uspješno izvršio SIP Digest autentikaciju i registraciju (REGISTER → 401 → REGISTER → 200 OK), što se vidi u IMS logu:

```bash
User-Agent: MicroSIP/3.22.3
...
SIP/2.0 401 Unauthorized
...
IMS -  - 1234 authenticated
IMS -  - Register 1234@192.168.200.180:63247
SIP/2.0 200 OK

```
Međutim, prilikom testiranja VoNR UE registracije (IMS/3GPP profil) IMS vraća grešku zbog sigurnosnih zahtjeva (sec-agree i ipsec-3gpp), te se u logu pojavljuje:
```bash
Unknown authent alg param in Security-Client header
SIP/2.0 420 Unsupported
Unsupported: sec-agree

```
Obzirom na ovo, izvršen je pokušaj onemogućavanja IPsec postavljanjem algoritama na "null":

```bash
/* IPSec */
ipsec_aalg_list: ["null"],
ipsec_ealg_list: ["null"],
```
Ovaj pokušaj nije dao očekivani rezultat – IMS/VoNR registracija i dalje nije u potpunosti funkcionalna, te je potrebno dodatno istražiti kako pravilno ukloniti ili zaobići IPsec zahtjeve (ili alternativno koristiti SIP klijent koji podržava 3GPP IPsec).




<div align="center">
  <img src="assets/5g/images/extensions_conf.png" alt="extensions_conf.png" title=" Prikaz extensions.conf konfiguracijske datoteke" style="width:75%">
  <br>
  <i>Slika 12: Prikaz <code>extensions.conf</code> konfiguracijske datoteke</i>
</div>

---

<details id="radni-paketi">
<summary title="Kliknite za prikaz radnih paketa.">Radni paketi</summary>
<table>
  <tr>
    <td><b>Radni paket</b></td>
    <td><b>Opis</b></td>
    <td><b>Plan isporuke</b></td>
  </tr>
  <tr>
    <td>RP1 - Dizajn koncepta FMC</td>
    <td>Definisati FMC koncept koji povezuje VoNR i IMS/SIP fiksne mreže uz scenarije zajedničkog IMS jezgra u 5G mreži, zajedničkog jezgra u fiksnoj mreži i odvojena IMS jezgra 5G i fiksne mreže.
    </td>
    <td>
      <ul>
        <li>Arhitekturni dijagrami</li>
        <li>Opis signalizacijskih tokova (visok nivo)</li>
        <li>Pretpostavke i ograničenja</li>
        <li>Lista potrebne opreme/softvera</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>RP2 - Implementacija VoNR usluge</td>
    <td>Implementacija VoNR korištenjem AMARI Callbox Mini rješenja i 5G mobilnih telefona.</td>
    <td>
      <ul>
        <li>Postavka testnog okruženja</li>
        <li>Registracija i uspostava poziva</li>
        <li>Evidencija konfiguracija</li>
        <li>Screenshotovi/logovi</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>RP3 - Implementacija FMC za scenarij (1)</td>
    <td>FMC za slučaj gdje je IMS jezgro zajedničko i nalazi se u 5G mreži.</td> 
    <td>
      <ul>
        <li>Implementacijski koraci</li>
        <li>Test scenariji</li>
        <li>Logovi signalizacije</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>RP4 - Implementacija FMC za scenarij (3)</td> 
    <td>FMC za slučaj gdje su IMS jezgra 5G i fiksne mreže odvojene.</td>
    <td>
      <ul>
        <li>Implementacijski koraci</li>
        <li>Test scenariji</li>
        <li>Logovi signalizacije</li>
      </ul>
    </td> 
  </tr>
  <tr>
   <td>RP5 - Eksperimentalna analiza signalizacijskih tokova</td> 
   <td>Analiza signalizacijskih tokova za scenarije (1) i (3).</td> 
  <td>
    <ul>
      <li>Definisati šta se tačno snima/posmatra</li>
      <li>Prikupiti logove</li>
      <li>Napraviti sekvencne dijagrame</li>
      <li>Zaključci i poređenje</li>
    </ul>
  </td>
  </tr>
  <tr>
   <td>RP6 - Repozitorij i dokumentacija</td> 
    <td>
      Održavanje strukture repozitorija, ažuriranje svih datotek ažuriranje svih datoteka i predlaganje ideja za poboljšanje postojećih datoteka. 
    </td>
    <td>
      <ul>
        <li>Redovno praćenje i održavanje strukture repozitorija</li>
        <li>Ažuriranje README.md datoteke</li>
      </ul>
    </td>
  </tr>
</table>
</details>

# Literatura:
[^1]: Raj, M., Narayan, A., Datta, S., Das, S. K., & Pathak, J. K. (2010). Fixed mobile convergence: challenges and solutions. IEEE Communications Magazine, 48(12), 26-34.
[^2]: What is Voice over New Radio (VoNR). NG-Voice <a href="https://www.ng-voice.com/learning-center/what-is-voice-over-new-radio-vonr#what-is-voice-over-new-radio-vonr">Link</a>
