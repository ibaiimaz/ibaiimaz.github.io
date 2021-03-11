---
title: "Berehala itzuli (return early)"
categories:
  - Parktika onak
tags:
  - Kodea
  - Parktika onak
  - Clean Code
---

Urteetan zehar software garatzaile askok uste izan du funtzio batek itzulera puntu bakar bat eduki behar lukeela, hau da, teknikoki esanda, *return* bakar bat egon behar lukeela funtzioko.

{% include toc %}

Uste honen zergatia oso argi ez badaukat ere, irakurri izan dut beharbada jatorria *C* programazio lengoaian egon litekeela. Gaur egun ere noski erabiltzen den arren, garai batean askoz zabalduagoa zegoen bere erabilera eta gaur egungo *Java, C#, Python* eta antzerako lengoaiekin ez bezala, *C*-n memoriaren kudeaketaz norbera arduratu behar zen eta beraz, itzulera puntu bakar bat edukitzeak asko errezten zuen kudeaketa hau guztia.

Urteak pasa dira eta egun, orokorrean, ez dugu horretaz kezkatu beharrik baina askok pentsatzen jarraitzen dute puntu bakar batean itzultzea praktika on bat dela.

Demagun sarreren prezioen deskontua kalkulatzen duen funtzio bat daukagula. Lehenik eta behin, deskontua izateko bazkide izan beharra dago eta horretaz gain adinaren arabera, deskontua ezberdin kalkulatzen da. Hau da, 26 urtez berakoek deskontu bat daukate eta 65 urtez gorakoek beste bat. Gainontzeko bazkideek oinarrizko deskontu bat daukate.

Itzulera puntu bakarraren kontzeptua jarraituz honelako zerbait edukiko genuke.

```javascript
function calculateDiscount (age, isMember) {
    let finalDiscount;

    if (isMember) {
        if (age < 26) {
            // gazteen deskontua kalkulatu
            ...
            finalDiscount = youthDiscount;
        } else if (age > 65) {
            // helduen deskontua kalkulatu
            ...
            finalDiscount = seniorsDiscount;
        } else {
            // oinarrizko deskontua kalkulatu
            ...
            finalDiscount = basicDiscount;
        }
    } else {
        finalDiscount = 0;
    }

    return finalDiscount;
}
```

Egiazko proiektu batean ziurrenik hau ez nuke honela egingo. Gure funtzioak erantzukizun bat baino gehiago dauka eta beraz *SOLID* printzipioetako *S*-a (Erantzukizun Bakarraren Printzipioa) urratzen ari gara. Hortaz gain, funtzio hau hedatu nahiko bagenu eta esaterako haurrentzat deskontu bat gehitu, funtzioa aldatu beharko genuke eta beraz, litekeena da, beste *SOLID* printzipioetako bat, *O*-a (Irekita-Itxita Printzipioa) ere urratzen aritzea. Baina adibide bat baino ez da eta beraz ez diogu horri erreparatuko.

Behin hau esanda, adibidera itzuliz, gertatzen da itzulerako aldagaia definitzen dugunetik, itzultzen dugun arte, toki ezberdinetatik pasatzen dela eta beraz, erreza dela bidean aldagai honi gertatzen zaionarekiko kontrola galtzea. Egia da ere, gure kodea geroz eta laburragoa eta garbiagoa izan, errezagoa izango dela pista jarraitzen, baina ez daukagu horrela ibiltzeko inongo beharrik.

Gainera, modu honetara eginaz, hau da, itzulera puntu bakar batekin eta deskonturako baldintza betetzen dela egiaztatuz, gure funtzioak indentazio maila bat baino gehiago dauka eta honek irakurgarritasuna zailtzen du.

# BEREHALA ITZULI (RETURN EARLY)

Berehala itzultzeak (edo *return early*) esan nahi du, izenak dioen bezalaxe, lortu beharreko balio izan bezain laister itzuli beharko genukeela.

Printzipio hau jarraituz, goiko adibidea beste modu honetara egin genezake, itzulera puntu bat baino gehiago erabiliz.

```javascript
function calculateDiscount (age, isMember) {
    if (isMember && age < 26) {
        // gazteen deskontua kalkulatu
        ...
        return youthDiscount;
    }
    
    if (isMember && age > 65) {
        // helduen deskontua kalkulatu
        ...
        return seniorsDiscount;
    }

    if (isMember) {
        // oinarrizko deskontua kalkulatu
        ...
        return basicDiscount;
    }

    return 0;
}
```

Aldaketa nabarmena dela uste dut, sinpletasunean asko irabazi dugu. Baldintza bakoitzak bere barnean kalkulatutako balioa itzultzen du zuzenean eta beraz ez dugu kezkatu beharrik horren ondoren egon daitekeen kodeaz.

Kanpoan definiturik geneukan aldagai orokorra ere kendu dugu eta beraz, kode lerro gutxiago izateaz gain, kodean zehar mutatuz doan aldagai bat edukitzea ekidin dugu. Hau beti izaten da problematikoa, zaila izaten bait da kontrolatzen haren gainean gertatzen diren aldaketa ezberdinak.

Gainera, defektuzko kasurako (hau da, inongo deskonturik aplikatzen ez denerako), zuzenean 0 itzultzen dugu eta kitto. Defektuzkoa kasua azken tokian itzultzeak ordea beste hiru baldintzetan hau kontutan izatera behartzen gaitu.

Kasu hauetarako (besteak beste) dator ongi hurrengo kontzeptua.

# BABES-KLAUSULA (GUARD CLAUSE)

Babes-klausula (edo *guard clause*) bat kontrol bat baino ez da, non baldintza baten arabera zuzenean funtziotik irteten den, dela `return` baten bitartez edo salbuespen bat altxatuz. `if` alderantzikatu bat dela ere esaten da, funtzioan exekutatu nahi dugun kodera iristeko baldintza hartu eta buelta ematen bait diogu, zein kasutan exekutatuko dugun esan ordez, zein kasutan ez dugun exekutatuko esaten bait dugu.

Aurreko adibidean esaterako, badugu baldintza bat, bete ezean, ez duguna inongo deskonturik aplikatuko, hau da, bazkide izatea, eta beraz, kasu horretan ez dugu inongo kalkulorik egin behar. Baldintza hau lehen tokian jarri genezake, buelta emanda, eta honela geldituko litzateke.

```javascript
function calculateDiscount (age, isMember) {
    if (!isMember) {
        return 0;
    }

    if (age < 26) {
        // gazteen deskontua kalkulatu
        ...
        return youthDiscount;
    }
    
    if (age > 65) {
        // helduen deskontua kalkulatu
        ...
        return seniorsDiscount;
    }

    // oinarrizko deskontua kalkulatu
    ...
    return basicDiscount;
}
```

Modu honetara gure baldintzak askoz sinpleagoak geratu dira, izan ere, lehen baldintzatik aurrera ziurtzat jo bait dezakegu deskontu bat aplikatu behar dela eta soilik zein deskontu aplikatu behar den erabaki behar da.

Lehen baldintza hori izango litzateke kasu honetan **babes-klausula**, funtzioak deskontuak kalkulatzen bait ditu, eta beraz, deskonturako baldintza bete ezean ez bait dauka zenturik funtzioa exekutatzen jarraitzeak.

Beste adibide bat ikusiko dugu **babes-klausula** bat nola aplikatu azaltzeko. Demagun puntu-komaz bananduriko zenbakiak parametro bidez jasotzen dituen funtzio bat daukagula eta honek zenbaki horiek guztiak batu behar dituela. Jasotzen dugun *string*-a hutsik baldin badago ordea, 0 bat itzuli behar dugu zuzenean.

Itzulera puntu bakarra erabiliz horrelako zerbait edukiko genuke.

```javascript
function sumStringNumbers (value) {
    let result;

    if (value) {
        const numbers = value.split(";");

        const numbersSum = 0;
        for (const number of numbers) {
            numbersSum += parseInt(number);
        }

        result = numbersSum;
    } else {
        result = 0;
    }

    return result;
}
```

Goiko adibidean, `for` erabili ordez nik egia esan `reduce` bat erabiliko nuke, baina adibiderako adierazgarriagoa dela pentsatu dut.
```javascript
    result = numbers.reduce((acc, number) => acc + parseInt(number), 0);
```

Itzulera puntu bakarra edukitzeak beste behin ere behartzen gaitu aldagai orokor bat edukitzera, eta beraz, honen balioa nundik datorren jakitea zailxeagoa da, baina ez hori bakarrik, gure logika nagusia `if` baten barnean jarri behar izan dugunez, jadanik indentazio maila batekin hasten gara. Ondoren `for`-ak ere beste indentazio bat gehitzen du, eta zenbat eta indentazio maila gehiago eduki, kodea ulertzen zailagoa izaten da beti.

Beti sar genezake gure logika nagusia funtzio baten barnean eta honela behintzat bigarren identazio maila hori kenduko genuke.

```javascript
function sumStringNumbers (value) {
    let result;

    if (value) {
        const numbers = value.split(";");
        result = sumNumbers(numbers);
    } else {
        result = 0;
    }

    return result;
}

function sumNumbers (numbers) {
    const numbersSum = 0;
    for (const number of numbers) {
        numbersSum += parseInt(number);
    }

    return numbersSum;
}
```

Hau noski hasierako kodea baino askoz hobea da, baina hala ta guztiz ere, aldagai orokor bat daukagu eta gure logika nagusiak baldintza baten barruan jarraitzen du.

Izan liteke ere itzulera puntu anitz erabiltzea baina hala ere, lehen baldintza horri buelta ez ematea, hau da, ez alderantzikatzea, eta beraz, tarteko bide batean geratuko ginateke non aldagai orokorraz desegin garen, baina gure logika nagusiak ez daukan behar duen protagonismoa.

```javascript
function sumStringNumbers (value) {
    if (value) {
        const numbers = value.split(";");
        return sumNumbers(numbers);
    } else {
        return 0;
    }
}
```

Honen aurrean berriro ere, ***babes-klausula*** bat erabil genezake.

Goiko adibidean egingo genukeena da, gure logika nagusia exekutatuko ez dugun kasuko baldintza jarri lehen tokian, hau da, jasotako balioa hutsik dagoen ala ez aztertzea. Hutsik egonez gero, defektuzko balioa itzuliko dugu zuzenean. Eta jarraian, inongo baldintzaren barruan sartu gabe, gure logika nagusia joango litzateke.

Hasierako kodea hartuta honela geratuko litzateke.

```javascript
function sumStringNumbers (value) {
    if (!value) {
      return 0;
    }

    const numbers = value.split(";");

    const numbersSum = 0;
    for (const number of numbers) {
        numbersSum += parseInt(number);
    }

    return numbersSum;
}
```

Ikusten dugun moduan jada ez dugu aldagai orokorraren beharrik eta gure logika nagusia lehen maila batean aurkitzen da. Gainera, nahiko esplizituki gelditzen da jasotako balioa hutsik egonez gero, defektuzko balio bat itzuliko dugula zuzenean, eta bestela, funtzioaren izenak berak esaten duena egingo dugula, hots, jasotako zenbakiak batu.

Bigarren birfaktorizazioa hartuz gero, oraindik txukunago geratzen da kodea.

```javascript
function sumStringNumbers (value) {
    if (!value) {
      return 0;
    }

    const numbers = value.split(";");
    return sumNumbers(numbers);
}

function sumNumbers (numbers) {
    const numbersSum = 0;
    for (const number of numbers) {
        numbersSum += parseInt(number);
    }

    return numbersSum;
}
```

Ikusten duzuen moduan, itzulera puntu bakar bat erabili ordez edo gure logika nagusia bete beharreko baldintza guztien barnean sartu ordez, ahal bezain azkarren itzuliz gero, indentazio mailak kenduko ditugu kode sinpleagoa eta ulergarriagoa lortuz.

Antzekoa gertatuko litzateke, baldintza baten arabera defektuzko balio bat itzuli ordez salbuespen bat altxa beharko bagenu.

Imagina dezagun emailak bidaltzen dituen funtzio bat daukagula, eta hau burutu ahal izateko, batetik baliozko helbide elektroniko bat behar dugula, eta bestetik mezu bat. Bi baldintza hauek bete ezean emaila ez da bidaliko.

```javascript
function sendMail (mailAddress, subject, message) {
    if (isValid(mailAddress)) {
        if (message) {
            return mailSender.send(mailAddress, subject, message);
        } else {
            throw new Error("Cannot send an empty message.");
        }
    } else {
        throw new Error("Email address is not valid.");
    }
}
```

Beste behin ere funtzioaren betekizun nagusia, hau da, emaila bidaltzea, oso izkutaturik gelditzen da balioztapen guztien barnean. Kode oso sinplea izanda ere, ez da guztiz argi gelditzen.

Hau saihesteko, beste behin ere, berehala itzultzera joko genuke eta kasu hontan gainera izen zehatz bat ematen zaio.

# BIZKOR FALLATU (FAIL FAST)

Berehala itzuli kontzeptuan erabat oinarriturik, **bizkor fallatu** (edo *fail fast*) teknikak, kodearen exekuzioa gelditzera eramango gaituzten baldintza guztiak hasieran jartzean datza. Honek gure kodea sendoagoa egiten du, errezagoa bait da baldintza guzti hauek hasierako puntuan kokatu eta betetzen ez den baldintza bakoitzeko zuzenean irtetea. Modu honetan, beharko ez lukeen kasuren batean logika nagusia exekutatzera iritsi bada, errezagoa da akatsa bilatzen.

Goiko email bidaltzearen adibidea hartuz, honela geldituko litzateke.

```javascript
function sendMail (mailAddress, subject, message) {
    if (!isValid(mailAddress)) {
        throw new Error("Email address is not valid.");
    }

    if (!message) {
        throw new Error("Cannot send an empty message.");
    }

    return mailSender.send(mailAddress, subject, message);
}
```

Uste dut aldea begi bistakoa dela, eta oraindik nabariagoa izango litzateke baldintza gehiago izango bagenitu. Pentsa dezagun emailaren gaia ere nahitaezkoa dela.

Hasierako ikuspegitik honela geldituko litzateke.

```javascript
function sendMail (mailAddress, subject, message) {
    if (isValid(mailAddress)) {
        if (message) {
            if (subject) {
                return mailSender.send(mailAddress, subject, message);
            } else {
                throw new Error("Subject cannot be empty.");
            }
        } else {
            throw new Error("Cannot send an empty message.");
        }
    } else {
        throw new Error("Email address is not valid.");
    }
}
```

Eta *fail fast* erabiliz ordea honela.

```javascript
function sendMail (mailAddress, subject, message) {
    if (!isValid(mailAddress)) {
        throw new Error("Email address is not valid.");
    }

    if (!message) {
        throw new Error("Cannot send an empty message.");
    }

    if (!subject) {
        throw new Error("Subject cannot be empty.");
    }

    return mailSender.send(mailAddress, subject, message);
}
```

Geroz eta baldintza gehiago, aldea nabariagoa egiten da.

Gainera hau horrela egiteak beste aukera bat ematen digu, izan ere, baldintza guztiak hasieran jarri ditugunez eta hauetako edozein bete ezean zuzenean salbuespen bat altxa eta exekuzioa bukatzen denez, hauek guztiak funtzio baten barnean sar genitzazke.

```javascript
function sendMail (mailAddress, subject, message) {
    validateParams(mailAddress, subject, message);

    return mailSender.send(mailAddress, subject, message);
}

function validateParams (mailAddress, subject, message) {
    if (!isValid(mailAddress)) {
        throw new Error("Email address is not valid.");
    }

    if (!message) {
        throw new Error("Cannot send an empty message.");
    }

    if (!subject) {
        throw new Error("Subject cannot be empty.");
    }
}
```

Honela erabat banantzen ditugu balioztapenak eta logika nagusiaren exekuzioa.

# ONDORIOAK

Gaur egun, lengoaia gehienetan behintzat, ez daukagu inongo beharrik itzulera puntu bakar bat erabiltzeko eta honek, **berehala itzuli** edo *return early* patroia erabiltzeko aukera ematen digu. Teknika honen barnean aurkitzen dira bai **babes-klausulak** (*guard clause*) eta baita **bizkor fallatu** (*fail fast*) ere. Hauei esker, kode sinpleago bat lortuko dugu, indentazio mailak kenduko ditugulako eta balioztapen eta defektuzko balioen kasuan, hauek hasieran jarriko ditugulako, funtzioaren bigarren zatia logika nagusirako utziz.

<br>
<br>

>**OHARRA**
>
>Kode adibideetan jarri ditudan komentarioak kodea ordezkatzeko ziren, hau da, bere tokian kodeak egon beharko luke, horregatik duade euskaraz, besteak beste. Badakizue ez naizela komentarioak jartzearen oso lagun, eta jartzekotan, ingelesez jarriko nintuzke.
