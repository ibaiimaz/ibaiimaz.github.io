---
layout: post
title: "TDD hastapena: FizzBuzz kata - 1. atala"
categories:
  - TDD
tags:
  - TDD
  - Test
  - Kata
  - Programazioa
  - Mocha
---

TDD hastapenen inguruko post-multzoari nolabaiteko amaiera emateko, Kode kata bat egin nahi nuen eta mekanika ahalik eta hobekien erakusteko bideo bat grabatzeko asmoa neukan, Live Coding moduko bat. Baina denbora aurrera doa eta bideoak egin gabe jarraitzen du. Bideoa nola grabatu, soinuaren kalitatea, ondorengo edizioa, beharrezko denbora aurkitzeko arazoa... oztopo gehiegi, beraz erabaki dut beti bezela idatziz egitea, eta bideoan erakutsi nahi nituen zenbait atal, irudi bidez erakustea.

Post-multzo honen aurreko atalak hemen aurki ditzakezu:
- [TDD hastapena: Testak](/tdd-hastapena-testak/)
- [TDD hastapena: TDD](/tdd-hastapena-tdd/)
- [TDD hastapena: Katak](/tdd-hastapena-katak/)

{% include toc %}

Izenburuan irakur daitekeen moduan, gaurkoan kode kata bat egingo dugu, *FizzBuzz* kata hain zuzen ere. *FizzBuzz* kata ziurrenik dagoen kata ezagunenetakoa da eta oso aproposa hasierako kata moduan.

Egia da katak honek, nire buruan behintzat, bi atal bezela dauzkala, eta horietako bat da bereziki aproposa iruditzen zaidana hasierako kata moduan. Izan ere, zenbait tokitan, katak soilik atal hori izango balu bezela egiten dute. Besterik gabe, ikus dezagun kataren enuntziatua eta pixkanaka aztertuz joango gara.

# Kataren enuntziatua

[Hemen](https://codingdojo.org/kata/FizzBuzz/) aurkitu daiteke kataren enuntziatu originala, baina laburbilduz honakoa dio.

> Imagina ezazu 11 urteko haur bat zarela, eta klasearen azken 5 minutuetan zure matematikako irakasleak erabaki duela klasea "dibertigarriagoa" egiteko "joku" bat burutzea. Azaltzen dizue txandaka ikasle bakoitza seinalatuz joango dela eta hark segidako hurrengo zenbakia esan beharko duela batetik hasita. Zati "dibertigarria" honakoa da: tokatzen zaizun zenbakia 3ren multiploa bada, zenbakiaren ordez "Fizz" esan behar duzu, zenbakia 5en multiploa bada berriz "Buzz" esan behar duzu, eta zenbakia 3 eta 5en multiploa bada "FizzBuzz".
>
> Emaitza aldez aurretik jakiteko, hurrengo klaserako zenbaki zerrenda osoa osatu behar duzu. Kontuan izan 33 ikasle zaretela eta ziurrenik 3 erronda egiteko denbora emango duela.
>
> Idatzi beraz programa bat 1etik 100 arteko zenbaki guztiak itzultzen dituena baina 3ren multiploen kasuan "Fizz" itzuliko duena, 5en multiploen kasuan "Buzz" itzuliko duena eta 3 eta 5en multiploen kasuan "FizzBuzz" itzuliko duela.
>

## Irteera adibidea
> 1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz 16 17 Fizz 19 Buzz ... etab 100 arte

## Enuntziatu laburbildua

> Idatzi programa bat 1 eta 100 arteko zenbakiak itzuliko dituena baina ondorengo baldintzak betez:
> 
> - 3ren multiploen kasuan, zenbakiaren ordez Fizz itzuliko du
> - 5en multiploen kasuan, zenbakiaren ordez Buzz itzuliko du
> - 3 eta 5en multiploen kasuan, zenbakiaren ordez FizzBuzz itzuliko du
> - Gainontzeko kasuetan zenbakia bera itzuliko du

Hau da beraz egingo dugun kataren enuntziatua eta kata honetan, aurrerago esan bezela, nik bi atal ikusten ditut. Helburua 1etik 100 arteko zenbakiak itzultzea bada ere, zenbaki bakoitza 3 edo/eta 5en multiploa den ala ez jakiteko, zenbaki bat jaso eta berau, edo dagokion "Fizz", "Buzz" edo "FizzBuzz" itzuliko duen funtzio bat sortuko nuke lehenik.

Baina hau kata bat da eta katak TDD erabiliz egiten dira beraz, aurrena test bat idatzi beharko genuke.

# Ohartarazpena

Kodearekin hasi aurretik aipatu nik JavaScript erabiliko dudala eta JavaScripten beharrezkoa ez den arren klaseak erabiliko ditudala, adibidea klaseak nahitaez erabili behar dituzten beste lengoaia askoren antzekoago gera dadin. Bestalde, test liburutegi bezela *Mocha* erabiliko dut, *Chai*-rekin konbinaturik asertzioetarako. Bide batez aipatu, abiapuntu moduan erabiliko dudan proiektua nire [Github](https://github.com/ibaiimaz/js-mocha-abiapuntua) kontuan daukazuela eskuragarri.

Proiektuko *Readme* fitxategian dioen bezela, terminalean jatorrizko direktoriora joan (package.json dagoen tokira) eta `npm install` exekutatuko dugu. Behin bukatutakoan, guztia ongi konfiguraturik dagoela ziurtatzeko testak exekutatuko ditugu `npm test` erabiliz.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/npm-install.gif' | absolute_url }}" alt=""></a>
</figure>

Ikus dezakegu abiapuntu proiektuak dakarren frogazko testa zuzen exekutatu dela eta berdean dagoela. Orain bai, kodea idazten hasteko garaia da.

Lehenik eta behin, frogazko test eta inplementazioa fitxategiak ezabatuko ditugu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/initial-files.png' | absolute_url }}" alt=""></a>
</figure>

# Kataren inplementazioa

Test fitxategi berri bat sortuko dugu. Aurrerago esan bezela, aurrena zenbaki bat eman eta aipatutako baldintzen arabera zenbakia bera, "Fizz", "Buzz" edo "FizzBuzz" itzuliko duen funtzio bat idatzi nahi dugu, horretarako *FizzBuzzer* deituko diodan klase bat sortuz, beraz, gure test fitxategia `FizzBuzzer.test.js` deituko da.

Fitxategi honetan gure lehen testa idatziko dugu.

```javascript
describe("FizzBuzzer", () => {
    it("should return Fizz if divisible by 3", () => {
        //Arrange - Given
        const fizzBuzzer = new FizzBuzzer();

        //Act - When
        const result = fizzBuzzer.parse(3);

        //Assert - Then
        expect(result).to.be.equal("Fizz");
    });
});
```

Testa exekutatzen dugu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/fizz-test-fail.png' | absolute_url }}" alt=""></a>
</figure>

Eta espero bezala fallatu egiten du. Gorrian gaude. Gogoratu hori dela *TDD*ren lehen pausoa. Kasu honetan falloa zuzenean klasea bera ere existitzen ez delako da, baina berdin dio, hori ere fallotzat hartzen da.

Beraz, `src` karpetaren barnean `FizzBuzzer.js` fitxategia sortuko dugu gure klase eta metodoarekin eta testa pasatu dadin beharrezkoa den gutxieneko kodea idatziko dugu.

```javascript
class FizzBuzzer {

    parse(number) {
        return "Fizz";
    }
}

module.exports = FizzBuzzer;
```
Kasu honetan, gutxieneko kode hori zuzenean `"Fizz"` itzultzea da. Hauek aurreko post batean aipatutako *baby steps*-ak dira.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/fizz-test-pass.png' | absolute_url }}" alt=""></a>
</figure>

Testa exekutatu eta berdean dago. Gure aurreneko testa idatzi dugu jada. Eman lezake zuzenean `"Fizz"` itzultzeak ez duela askorako balio, baina katako lehen baldintza beteko dela ziurtatzen digun lehenengo testa idatzirik daukagu eta orain bigarren baldintzaren bila joan gaitezke jakiniz bai hau eta bai aurrekoa bete beharko ditugula, baina ez hori soilik, baizik eta hori egiten dugula ziurtatuko duten testak dauzkagula.

Aurreko post batean aipatutako moduan, eskakizunekin zerrenda bat osatzea da egokiena, horrela testak idazten goazen arabera hauek zerrendatik kendu ditzakegu. Kasu honetan, gure eskakizunen zerrenda *Enuntziatu laburbilduan* daukagu eta gogoratuko duzuen moduan ondokoa da.

> Idatzi programa bat 1 eta 100 arteko zenbakiak itzuliko dituena baina ondorengo baldintzak betez:
> 
> - 3ren multiploen kasuan, zenbakiaren ordez Fizz itzuliko du
> - 5en multiploen kasuan, zenbakiaren ordez Buzz itzuliko du
> - 3 eta 5en multiploen kasuan, zenbakiaren ordez FizzBuzz itzuliko du
> - Gainontzeko kasuetan zenbakia bera itzuliko du

1etik 100 arteko zenbakiak itzultzearena bera eskakizun bat da, baina gu orain zenbakiak *parseatuko* dituen funtzioan murgildurik gaude eta beraz beste lau puntuak dira inporta zaizkigunak. Lehenengoa jada bete dugu.

Zerrenda bat egin ordez, *Mocha*rekin (eta ziur beste liburutegi askorekin ere) honakoa egin daiteke. Testak zuzenean idatzirik utz daitezke, baina testean bertan `skip` hitza idatziz hauek ez dira exekutatuko eta pendiente bezela azalduko dira.

Hau izango litzateke kode osoa.

```javascript
const FizzBuzzer = require('../src/FizzBuzzer');

describe("FizzBuzzer", () => {
    it("should return Fizz if divisible by 3", () => {
        const fizzBuzzer = new FizzBuzzer();

        const result = fizzBuzzer.parse(3);

        expect(result).to.be.equal("Fizz");
    });

    it.skip("should return Buzz if divisible by 5", () => {

    });

    it.skip("should return FizzBuzz if divisible by 3 and 5", () => {

    });

    it.skip("should return the number if not divisible by 3 nor 5", () => {

    });
});
```

Eta hau testak exekutatzerakoan ikusiko genukeena.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/fizz-test-skip.png' | absolute_url }}" alt=""></a>
</figure>

Modu honetan, ondoren inplementatu beharko ditugun eskakizunak zerrendaturik dauzkagu *test* moduan, baina ez diete gure testen exekuzioari eragiten. Ez dakit *TDD* kontuetan oso purista direnek zer pentsatuko ote duten teknika honi buruz, teorian testak banan banan idatzi behar direlako, baina azken finean, ez gera gure testen exekuziora aldi bakoitzean test bat baino gehiago gehitzen ari, soilik beren deskribapenak idazten baizik.

Nik nire egunerokoan batzutan erabiltzen dut teknika hau ta beste batzuetan ez, beraz, zuen esku uzten det, baina jakin dezazuela aukera existitzen dela.

Oraingo honetan utzi egingo ditut, iruditzen bait zait argiago gelditzen dela zer burutu dugun jada eta zer ez, eta bide batez, denbora guztian izango dugu begi bistan eskakizunen zerrenda.

Behin hau guztia esanda, hurrengo testa idazteko garaia da, hau da, pasatako zenbakia 5en multiploa bada *Buzz* itzultzea.

```javascript
it("should return Buzz if divisible by 5", () => {
    const fizzBuzzer = new FizzBuzzer();

    const result = fizzBuzzer.parse(10);

    expect(result).to.be.equal("Buzz");
});
```

Eta testak exekutatuko ditugu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/buzz-test-fail.png' | absolute_url }}" alt=""></a>
</figure>

Eta espero bezala fallatu egiten du gure kodeak beti *Fizz* itzultzen duelako konstante moduan.

Testa pasa arazteko idatz genezakeen gutxieneko kodea honakoa izan liteke adibidez.

```javascript
class FizzBuzzer {

    parse(number) {
        if (number % 3 === 0) {
            return "Fizz";
        }

        return "Buzz";
    }
}
```

Bai aurreko testak eta bai berriak pasatzen jarraitu behar dutenez, lehenengo testa (eta honek dakarren eskakizuna) betetzeko beharrezkoa den egiazko kodea idatz genezake, hau da, jasotako zenbakia 3gatik zatitzean hondarra 0 dela egiaztatzea, eta test berria berdean ahalik eta azkarren jartzeko, gainontzeko kasu guztietarako *Buzz* itzuliko dugu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/buzz-test-pass.png' | absolute_url }}" alt=""></a>
</figure>

Hurrengo testarekin goaz jarraian. Pasatako zenbakia 3 eta 5en multiploa bada *FizzBuzz* itzuli behar dugu.

```javascript
it("should return FizzBuzz if divisible by 3 and 5", () => {
    const fizzBuzzer = new FizzBuzzer();

    const result = fizzBuzzer.parse(15);

    expect(result).to.be.equal("FizzBuzz");
});
```

Eta testak noski fallatu egiten du.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/fizzbuzz-test-fail.png' | absolute_url }}" alt=""></a>
</figure>

Lehen egindako berdina egiteko tentazioa eduki genezake, hau da, *Buzz* itzultzeko beharrezkoa den logika idaztea eta *FizzBuzz* gainontzeko kasuetarako defektuzko balore bezela uztea.

```javascript
class FizzBuzzer {

    parse(number) {
        if (number % 3 === 0) {
            return "Fizz";
        }

        if (number % 5 === 0) {
            return "Buzz";
        }

        return "FizzBuzz";
    }
}
```

Baina testek emandako errorea begiratzen badugu ikusiko dugu ez duela *Buzz* itzuli (hau da defektuzko balorea), baizik eta *Fizz* itzuli duela, izan ere, 3 eta 5 zenbakien multiploen kasua idazten ari gara eta beraz, lehenengo baldintzatik sartzen ari da. Alde ona da, testei esker errez ohartu ahal izan garela, hauek izan ezean, baldintza hori azken tokian jar genezake eta behar bada ez konturatu zuzenean lehenengo baldintzatik irteten ari garela.

Hau guztia esanik beraz, test guztiak pasatzeko idatz dezakegun gutxieneko kodea honakoa da.

```javascript
class FizzBuzzer {

    parse(number) {
        if (number % 3 === 0 && number % 5 === 0) {
            return "FizzBuzz";
        }

        if (number % 3 === 0) {
            return "Fizz";
        }

        return "Buzz";
    }
}
```

Eta honekin testak berdean dauzkagu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/fizzbuzz-test-pass.png' | absolute_url }}" alt=""></a>
</figure>

Hona iritsita, ez dugu ahaztu behar *TDD*k hirugarren pauso bat ere baduela: *Red*, *Green*, *Refactor*. Beraz, birfaktorizatzeko zerbait ote dugun begiratzea ondo legoke. Eta kontuan izan, ez dugula soilik gure produkzioa kodea birfaktorizatu behar, testak ere ahalik eta *garbien* mantendu behar ditugu.

Hau esanik, gure testak begiratzen baditugu, oso ohikoa den birfaktorizazio batekin aurkituko gara.

```javascript
const FizzBuzzer = require('../src/FizzBuzzer');

describe("FizzBuzzer", () => {
    it("should return Fizz if divisible by 3", () => {
        const fizzBuzzer = new FizzBuzzer();

        const result = fizzBuzzer.parse(3);

        expect(result).to.be.equal("Fizz");
    });

    it("should return Buzz if divisible by 5", () => {
        const fizzBuzzer = new FizzBuzzer();

        const result = fizzBuzzer.parse(10);

        expect(result).to.be.equal("Buzz");
    });

    it("should return FizzBuzz if divisible by 3 and 5", () => {
        const fizzBuzzer = new FizzBuzzer();

        const result = fizzBuzzer.parse(15);

        expect(result).to.be.equal("FizzBuzz");
    });

    it.skip("should return the number if not divisible by 3 nor 5", () => {

    });
});
```

Behin eta berriz *FizzBuzzer* klasea instantziatzen ari gara. Kasu honetan ez dauka konplexidade gehiegi, ez bait diogu inongo parametrorik pasatzen eraikitzaileari, baina praktika ona da dena den hau kanpora ateratzea.

```javascript
const FizzBuzzer = require('../src/FizzBuzzer');

describe("FizzBuzzer", () => {
    let fizzBuzzer;

    beforeEach(() => {
        fizzBuzzer = new FizzBuzzer();
    });

    it("should return Fizz if divisible by 3", () => {
        const result = fizzBuzzer.parse(3);

        expect(result).to.be.equal("Fizz");
    });

    it("should return Buzz if divisible by 5", () => {
        const result = fizzBuzzer.parse(10);

        expect(result).to.be.equal("Buzz");
    });

    it("should return FizzBuzz if divisible by 3 and 5", () => {
        const result = fizzBuzzer.parse(15);

        expect(result).to.be.equal("FizzBuzz");
    });

    it.skip("should return the number if not divisible by 3 nor 5", () => {

    });
});
```

*Mocha*ren *BeforeEach* funtzioa erabili dugu horretarako. Hau, test bakoitzaren aurretik exekutatzen da.

Eta noski, birfaktorizazio bakoitzaren ondoren testak exekutatu behar ditugu, eta kasu hontan, denak berdean jarraitzen du.

Azken eskakizunari heltzeko unea iritsi da.

```javascript
it("should return the number if not divisible by 3 nor 5", () => {
    const result = fizzBuzzer.parse(2);

    expect(result).to.be.equal("2");
});
```

Eta espero bezela testa ez da pasa.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/default-test-fail.png' | absolute_url }}" alt=""></a>
</figure>

Lehengo mekanikarekin jarraituz, orain bai, *Buzz* itzultzeko baldintza idatziko genuke eta gainontzeko guztietan, defektuzko balio bezela jasotako zenbakia bera, *string* bilakatuta.

```javascript
class FizzBuzzer {

    parse(number) {
        if (number % 3 === 0 && number % 5 === 0) {
            return "FizzBuzz";
        }

        if (number % 3 === 0) {
            return "Fizz";
        }

        if (number % 5 === 0) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Orain bai, test guztiak berdean daude.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/default-test-pass.png' | absolute_url }}" alt=""></a>
</figure>

Honekin, hasieran aipatutako lehen zatia bukaturik egongo litzateke, hau da, edozein zenbaki emanik eta aipatutako eskakizunak betez, zenbakia bera, *Fizz*, *Buzz* edo *FizzBuzz* itzuliko digun funtzio bat daukagu.

Gehiegi ez luzatzeko, bigarren zatia, hau da, 1 eta 100 arteko zenbakiak *parse*aturik itzultzea, bigarren atal baterako utziko dugu.

Bide batez esan hurrengo post batean, kodea zati honen gainean egingo nukeen brifaktorizazio txiki baten inguruan hitz egingo duala.