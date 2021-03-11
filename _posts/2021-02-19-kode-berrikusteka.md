---
layout: post
title: "Kode atal baten berrikusketa"
categories:
  - Kode Berrikusketa
tags:
  - Kodea
  - Kode Berrikusketa
  - Clean Code
---

Gaurkoan kode atal baten berrikusketarekin natorkizue. Asmoa, berau nola aldatuko nukeen, eta zergaitik azaltzea da.


{% include toc %}

Aurreko batean, jarraian jarriko dudan kode zatia aurkitu nuen interneten nenbilela. Ez dauka garrantzirik nundik atera nuen eta are gehiago, elkarbanatu zuenaren asmoa ziurrenik ez zen kodea bera erakustea, egoera jakin bat isladatzea baizik argazki baten bitartez. Baina nire kuriositateak zoom egin eta kodea begiratzera eraman ninduen, eta nire deformazio profesionalak, nik neuk nola idatziko nukeen pentsatzera.

Aurrerago jakin dut (behin post osoa idatzirik neukala), ariketa baten zati bat zela eta zenbait baldintza bete behar zituela, horregaitik, besteak beste, zegoen dena funtzio bakar batean idatzirik. 

Baina nire asmoa ez denez kodearen gaineko kritika bat egitea baizik eta abiapuntu moduan erabili eta nik nola birfaktorizatuko nukeen azaldu eta zenbait hausnarketa egitea, berdin berdin erabiliko dut. Bide batez esan, garrantzirik ez badauka ere, kodea *Javan* idatzirik dagoela.

# HASIERAKO KODEA

Ondoko hau litzateke kodearen abiapuntua. Prezio bat itzultzen duen funtzio bat.

```java
public int precioFinal() {
    int calculoFinal = this.getPrecioBase();
    //Para el Certificado energetico
    switch (this.getCEnergetico()) {
        case 'A':
            calculoFinal =calculoFinal+100;break;
        case 'B':
            calculoFinal =calculoFinal+80;break;
        case 'C':
            calculoFinal =calculoFinal+60;break;
        case 'D':
            calculoFinal =calculoFinal+50;break;
        case 'E':
            calculoFinal =calculoFinal+30;break;
        case 'F':
            calculoFinal =calculoFinal+10;break;
    }
    //Para el peso
    if ((this.getPeso()>=0)&&(this.getPeso()<=19)) {
        calculoFinal =calculoFinal+10;
    }
    if ((this.getPeso()>=20)&&(this.getPeso()<=49)) {
        calculoFinal =calculoFinal+50;
    }
    if ((this.getPeso()>=50)&&(this.getPeso()<=79)) {
        calculoFinal =calculoFinal+80;
    }
    if ((this.getPeso()>=80)) {
        calculoFinal =calculoFinal+100;
    }
    return calculoFinal;
}
```

Kode honengan egingo nukeen lehen aldaketa, guztia ingelesera itzultzea litzateke.

# ZEIN HIZKUNTZATAN IDATZI KODEA

Nik egun ez daukat inongo zalantzarik: kodea ingelesez. Asko dira arrazoiak. Nagusiena, ingelesa dela software munduko hizkuntza *ofiziala*. Dena dago ingelesez idatzirik, behar bada ez soilik ingelesez, baina ingelesez ere. Kontraesana eman lezake, euskara software munduan sustatzeko blog bat sortu duen pertsona baten *ahotik*, baina horrela da, ezin ukatu.

Dokumentazio ofizial guztiak ingelesez daude, eta software libreko proiektu guztien kodea ere. Beraz, gure kodea noizbait software libre moduan zabalduko badugu, ingelesez idatzi behar du egon.

Lan egin izan dut kodea gazteleraz idazten zuten enpresa bat baino gehiagotan. Sekula ez nuen arrazoia galdetu, baina beti suposatu izan dut hasi zuenak modu naturalean *bere hizkuntzan* idaztea erabaki zuela, edo behintzat enpresako hizkuntza *ofizialean*. Nire kasuan nere hizkuntza euskara da, baina badakit tamalez, enpresa haietan nire lankide guztiak ez liratekeela gai izango modu egokian ulertzeko, eta beraz, hizkuntza unibertsalago batean idazteak zentzua daukala. Baina noski, hizkuntza unibertsal bat aukeratzen hasita, zer unibertsalago ingelesa baino?

Bestalde, egun erremoto bidezko lanekin, geroz eta ohikoagoa da jatorri ezberdineko pertsonak dauden taldeetan lan egitea, edo oraintxe ez bada ere, etorkizun batean egitea, beraz zentzu guztia dauka ingelesa aukeratzea koderako hizkuntza bezala.

Azkenik, *spanglisharen* kontua dago. Zenbait aditz oso barneratuak daude ingelesez, adibide argienak `get` eta `set` dira. Are gehiago, zenbait programazio lengoaiatan, konbentzio moduan erabiltzen dira, eta beraz, adibide hontakoa bezelako `getPrecioBase` edo `getPeso` bezelako hitzekin aurkitzen gara. Gainera, esan beharra dago orokorrean ingelesez letra gutxiago behar direla gazteleraz, eta batez ere euskeraz, zerbait adierazteko baino, eta kontuan izanda gure kodeak ahalik eta espresiboena izan behar duela, gutxiagorekin gehiago esateko gai izango gara.

Honela geldituko litzateke kodea.

```java
public int totalPrice() {
    int finalCalculation = this.getBasePrice();
    //Para el Certificado energetico
    switch (this.getEnergyCertificate()) {
        case 'A':
            finalCalculation =finalCalculation+100;break;
        case 'B':
            finalCalculation =finalCalculation+80;break;
        case 'C':
            finalCalculation =finalCalculation+60;break;
        case 'D':
            finalCalculation =finalCalculation+50;break;
        case 'E':
            finalCalculation =finalCalculation+30;break;
        case 'F':
            finalCalculation =finalCalculation+10;break;
    }
    //Para el peso
    if ((this.getWeight()>=0)&&(this.getWeight()<=19)) {
        finalCalculation =finalCalculation+10;
    }
    if ((this.getWeight()>=20)&&(this.getWeight()<=49)) {
        finalCalculation =finalCalculation+50;
    }
    if ((this.getWeight()>=50)&&(this.getWeight()<=79)) {
        finalCalculation =finalCalculation+80;
    }
    if ((this.getWeight()>=80)) {
        finalCalculation =finalCalculation+100;
    }
    return finalCalculation;
}
```

Egingo nukeen hurrengo gauza komentarioak kentzen saiatzea litzateke.

# KOMENTARIOAK KODEAN BAI ALA EZ

Hemen erantzuna ez da hain xinplea. Kasuaren arabera. Ni orokorrean kodean komentariorik ez jartzearen aldekoa naiz. Ez behintzat gauza hori bera kode bidez adierazi dezakedala uste badut.

Egia da hau beti ez dela hain erreza. Programatzerakoan, gezurra badirudi ere, gauzarik zailenetako bat gauzei izena ematea da. Izena ematea baino, izen egokia ematea. Helburua, kodea irakurtze soilarekin zer egiten duen ulertzera iristea litzateke, eta horretan, beste gauza batzuen artean, izenek berebiziko garrantzia daukate. Baina beharbesteko denbora eskeintze badiogu, praktika pixka batekin, iritsi gaitezke ulertu ahal izateko komentarioen laguntzarik beharko ez duen kode bat izatera.

Batzuetan hala ere beharko ditugu komentarioak, baina orokorrean, nere ikuspuntutik behintzat, komentario horiek ez lukete izan behar **zer** egiten ari garen azaltzeko, hori kodeak berak adierazi beharko luke, **zergatik** ari garen egiten azaltzeko baizik.

Komentarioen beste arazoetako bat zaharkiturik gelditu daitezkeela (eta orokorrean gelditzen direla) da. Eta beraz, gerta liteke komentarioak gauza bat esatea eta kodeak, denboraren poderioz, beste gauza ezberdin bat egitea. Honen arazo haundiena da, komentarioetaz ez fidatzen bukatzen dugula, eta beraz, ezer gutxi ematen digutela.

Adibide konkretu honetan bi komentario daude, hain zuzen ere, kodeak egiten dituen bi zeregin nagusiak izendatzeko. Komentario horiek kentzeko modurik zuzenena eginkizun horietako bakoitzari dagokion kode zatia funtzio pribatu banatara ateratzea litzateke.

Energia zertifikatuaren prezioa aplikatzeari dagokion kodea honela geldituko litzateke.

```java
public int totalPrice() {
    int finalCalculation = this.getBasePrice();
    finalCalculation = finalCalculation + getEnergyCertificatePrice();

    //Para el peso
    if ((this.getWeight()>=0)&&(this.getWeight()<=19)) {
        finalCalculation =finalCalculation+10;
    }
    if ((this.getWeight()>=20)&&(this.getWeight()<=49)) {
        finalCalculation =finalCalculation+50;
    }
    if ((this.getWeight()>=50)&&(this.getWeight()<=79)) {
        finalCalculation =finalCalculation+80;
    }
    if ((this.getWeight()>=80)) {
        finalCalculation =finalCalculation+100;
    }
    return finalCalculation;
}

private int getEnergyCertificatePrice() {
    switch (this.getEnergyCertificate()) {
        case 'A':
            return 100;
        case 'B':
            return 80;
        case 'C':
            return 60;
        case 'D':
            return 50;
        case 'E':
            return 30;
        case 'F':
            return 10;
    }
}
```

Eta pisuaren araberako prezioa aplikatzearena beste honela.

```java
public int totalPrice() {
    int finalCalculation = this.getBasePrice();
    finalCalculation = finalCalculation + getEnergyCertificatePrice();
    finalCalculation = finalCalculation + getWeightPrice();

    return finalCalculation;
}

private int getEnergyCertificatePrice() {
    switch (this.getEnergyCertificate()) {
        case 'A':
            return 100;
        case 'B':
            return 80;
        case 'C':
            return 60;
        case 'D':
            return 50;
        case 'E':
            return 30;
        case 'F':
            return 10;
    }
}

private int getWeightPrice() {
    if ((this.getWeight()>=0)&&(this.getWeight()<=19)) {
        return 10;
    }
    if ((this.getWeight()>=20)&&(this.getWeight()<=49)) {
        return 50;
    }
    if ((this.getWeight()>=50)&&(this.getWeight()<=79)) {
        return 80;
    }
    if ((this.getWeight()>=80)) {
        return 100;
    }
}
```

Orain zeregin bakoitza funtzio propio batean dago eta beraz, batetik kodea banantzea lortu dugu funtzio nagusia, hots, `totalPrice` sinplifikatuz, eta bestetik kodeari adierazgarritasun askoz haundiagoa eman diogu, funtzio bakoitzaren izenak argi azaltzen bait digu funtzio horrek zer egiten duen, komentarioen beharrik gabe.

# ALDAEZINTASUNA EDO INMUTABILITATEA

Inmutabilitatea nahiko hitz potoloa da, egun nahiko modan dagoena, baina ez naiz gehiegi sartuko egia esan. Soilik azalpen azkar bat.

Ikusiko zenuten moduan, ez naiz kodea funtzioetara mugitzera mugatu. Funtzio bakoitzak orain dagokion prezioa `finalCalculation`-era gehitu ordez, prezio hori bera itzultzen du. Izan ere, kodea zegoen bezela mugitu izan banu, emaitza honelako zerbait litzateke.

```java
public int totalPrice() {
    int finalCalculation = this.getBasePrice();
    finalCalculation = getEnergyCertificatePrice(finalCalculation);
    finalCalculation = getWeightPrice(finalCalculation);

    return finalCalculation;
}

private int getEnergyCertificatePrice(int finalCalculation) {
    switch (this.getEnergyCertificate()) {
        case 'A':
            return finalCalculation + 100;
        case 'B':
            return finalCalculation + 80;
        case 'C':
            return finalCalculation + 60;
        case 'D':
            return finalCalculation + 50;
        case 'E':
            return finalCalculation + 30;
        case 'F':
            return finalCalculation + 10;
    }
}

    // code omitted for brevity
```

Nun aldagai bat parametro moduan pasatzen ari garen, bere balorea modifikatu eta berau itzultzeko. Aldrebes xamarra da. Baina orain egiten ari garena ez da askoz hobea.

```java
public int totalPrice() {
    int finalCalculation = this.getBasePrice();
    finalCalculation = finalCalculation + getEnergyCertificatePrice();
    finalCalculation = finalCalculation + getWeightPrice();

    return finalCalculation;
}
    // code omitted for brevity
```

Azken finean, `finalCalculation` aldagaiaren balioa aldatzen ari gara denbora guztian. Funtzioak modu honetara birfaktorizatu izanak ordea ondorengoa egitea ahalbidetzen digu.

```java
public int totalPrice() {
    int energyCertificatePrice = getEnergyCertificatePrice();
    int weightPrice = getWeightPrice();
    int finalCalculation = this.getBasePrice() + energyCertificatePrice + weightPrice;

    return finalCalculation;
}

// code omitted for brevity
```

Modu honetara `finalCalculation`-en balioa ez da aldatzen behin esleituta (ezta beste aldagaiena ere), inmutable izatera pasatuz. JavaScript erabiltzen ariko bagina, `let` erabiltzetik `const` erabiltzera pasa bagina bezela da. Java lengoaian `final` hitz gakoarekin ere berdina lor daiteke, baina JavaScript munduan praktika orokor moduan zabaldurik dagoen bitartean, ez det uste Javan honela gertatzen denik.

Adibidearekin jarraituz, kasu honetan, kode blokea oso laburra denez ez dauka beharbada hainbeste garrantzi, baina kode zati haundiagoetan, non aldagaia definitzen denetik une jakin batetara iritsi arte tartean zenbat mutazio izan dituen ikustea zaila izan daitekeen, oso lagungarria da zure objektuak aldaezinak direla jakitea.

Hau noski gehiago labur daiteke nahi izanez gero baina hori ja neurri batean gustu kontua da. Honela gelditu ahalko litzateke.

```java
public int totalPrice() {
    return this.getBasePrice() + getEnergyCertificatePrice() + getWeightPrice();
}

// code omitted for brevity
```

# KODE ABSTRAKZIOA

Azken aldaketa bat egingo nuke kode honetan. Funtzio honek, bere eginbeharra betetzen duen arren, ulergarritasun aldetik ez nau guztiz konbentzitzen.

```java
private int getWeightPrice() {
    if ((this.getWeight()>=0)&&(this.getWeight()<=19)) {
        return 10;
    }
    if ((this.getWeight()>=20)&&(this.getWeight()<=49)) {
        return 50;
    }
    if ((this.getWeight()>=50)&&(this.getWeight()<=79)) {
        return 80;
    }
    if ((this.getWeight()>=80)) {
        return 100;
    }
}
```

Azken finean, inporta zaidan gauza bakarra prezio bakoitzerako pisu tartea zein den definitzea da. Hau honela izanik, nire baldintzapeneko egituretako kondizioak ahalik eta ulerterrezenak izatea nahi dut, eta horretarako, ondoko aldaketak asko sinplifikatzen duela uste dut.

```java
private int getWeightPrice() {
    if (isWeightBetween(0, 19)) {
        return 10;
    }
    if (isWeightBetween(20, 49)) {
        return 50;
    }
    if (isWeightBetween(50, 79)) {
        return 80;
    }
    return 100;
}

private boolean isWeightBetween(int lowerLimit, int upperLimit) {
    return this.getWeight() >= lowerLimit && this.getWeight() <= upperLimit;
}
```

Honela, modu espresibo batean baldintzak zer aztertzen duen eta kontuan zein balio hartzen dituen ikus daiteke. Azken finean, niri berdin zait balio bat tarte jakin batean dagoen aztertzeko lengoaia jakin batean nola egin behar den ikustea. Horretaz ari naiz kode abstrakzioa esaten dudanean. Niri `isWeightBetween` funtzioak balio jakin bat nik ematen diodan tartean aurkitzen den ala ez esango dit, eta bost axola nola egiten duen.

# EMAITZA FINALA

Aldaketa guzti hauekin, emaitza finala honakoa litzateke.

```java
public int totalPrice() {
    return this.getBasePrice() + getEnergyCertificatePrice() + getWeightPrice();
}

private int getEnergyCertificatePrice() {
    switch (this.getEnergyCertificate()) {
        case 'A':
            return 100;
        case 'B':
            return 80;
        case 'C':
            return 60;
        case 'D':
            return 50;
        case 'E':
            return 30;
        case 'F':
            return 10;
    }
}

private int getWeightPrice() {
    if (isWeightBetween(0, 19)) {
        return 10;
    }
    if (isWeightBetween(20, 49)) {
        return 50;
    }
    if (isWeightBetween(50, 79)) {
        return 80;
    }
    return 100;
}

private boolean isWeightBetween(int lowerLimit, int upperLimit) {
    return this.getWeight() >= lowerLimit && this.getWeight() <= upperLimit;
}
```

Birfaktorizazio guzti hau noski, ezer hausten ez dugula ziurtatuko diguten test batzuk izanda egiten badugu askoz hobe. Beraz, testen inguruan gehiago jakin nahi izanez gero [hemen](/tdd-hastapena-testak/) eta [hemen](/tdd-hastapena-tdd/) informazio gehiago aurki dezakezu.
