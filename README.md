# SSMTK_FMC_VoNR_IMS_5G
Dizajn i implementacija fiksno-mobilne konvergencije (FMC) koja povezuje VoNR u 5G mreži sa IMS/SIP govornom uslugom u fiksnoj mreži, kroz više scenarija IMS jezgra.
Predmet: Sistemi i servisi mobilnih telekomunikacija  
Ak. godina: 2025/2026
Tim:
- Emina Hasković
- Zakira Jašarević
- Lejla Porobić
- Amr Saračević
- Muhamed Crnčalo
- Merjema Varupa

Alokacija radnih paketa

| Član tima        | RP1 | RP 2|
|------------------|-----|-----|
| Zakira Jašarević | RP1 | RP5 |
| Lejla Porobić    | RP1 | RP4 |
| Amr Saračević    | RP2 | RP3 |
| Emina Hasković   | RP3 | RP5 |
| Muhamed Crnčalo  | RP2 | RP5 |
| Merjema Varupa   | RP4 | RP6 |

## Radni paketi

### RP1: Dizajn koncepta FMC
**Zaduženi:** Zakira, Lejla  
**Opis:** Definisati FMC koncept koji povezuje VoNR i IMS/SIP fiksne mreže uz scenarije:
1) zajedničko IMS jezgro u 5G mreži  
2) zajedničko IMS jezgro u fiksnoj mreži  
3) odvojena IMS jezgra 5G i fiksne mreže

**Plan isporuke**
- [ ] Arhitekturni dijagrami
- [ ] Opis signalizacijskih tokova (visok nivo)
- [ ] Pretpostavke i ograničenja
- [ ] Lista potrebne opreme/softvera

**Rezultati**
- (linkovi na dijagrame/dokumente)

---

### RP2: Implementacija VoNR usluge
**Zaduženi:** Amr, Muhamed  
**Opis:** Implementacija VoNR korištenjem AMARI Callbox Mini rješenja i 5G mobilnih telefona.

**Plan isporuke**
- [ ] Postavka testnog okruženja
- [ ] Registracija i uspostava poziva
- [ ] Evidencija konfiguracija
- [ ] Screenshotovi/logovi

**Rezultati**
- (linkovi)

---

### RP3: Implementacija FMC za scenarij (1)
**Zaduženi:** Emina, Amr  
**Opis:** FMC za slučaj gdje je IMS jezgro zajedničko i nalazi se u 5G mreži.

**Plan isporuke**
- [ ] Implementacijski koraci
- [ ] Test scenariji
- [ ] Logovi signalizacije

**Rezultati**
- (linkovi)

---

### RP4: Implementacija FMC za scenarij (3)
**Zaduženi:** Merjema, Lejla  
**Opis:** FMC za slučaj gdje su IMS jezgra 5G i fiksne mreže odvojene.

**Plan isporuke**
- [ ] Implementacijski koraci
- [ ] Test scenariji
- [ ] Logovi signalizacije

**Rezultati**
- (linkovi)

---

### RP5: Eksperimentalna analiza signalizacijskih tokova
**Zaduženi:** Zakira, Emina, Muhamed
**Opis:** Analiza signalizacijskih tokova za scenarije (1) i (3).

**Plan isporuke**
- [ ] Definisati šta se tačno snima/posmatra
- [ ] Prikupiti logove
- [ ] Napraviti sekvencne dijagrame
- [ ] Zaključci i poređenje

**Rezultati**
- (linkovi)

---

## RP6: Repo & dokumentacija
**Zaduženi:** Merjema  
- Održavanje strukture repozitorija
- Ažuriranje README-a
- Otvaranje i praćenje Issues
- Sedmični “status update”

## Struktura repozitorija (prijedlog)

/docs  
  /rp1-design  
  /rp3-fmc-s1  
  /rp4-fmc-s3  
  /rp5-analysis  

/config  
/scripts  
/assets  

## Dnevnik promjena
- 2025-12-09: Kreiran repo i inicijalni README.

