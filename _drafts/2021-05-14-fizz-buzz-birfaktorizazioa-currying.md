---
layout: post
title: "FizzBuzz kataren \"currying\" birfaktorizazioa"
categories:
  - TDD
tags:
  - TDD
  - Test
  - Kata
  - Programazioa
  - Mocha
---

TDD hastapenen inguruko post-multzoko aurreko atalean *Fizz Buzz* kataren lehen zati bat burutu genuen. Gaurkoan, kode horren gainean birfaktorizazio txiki bat burutuko dugu *Currying* edo *curry*fikazio teknika erabiliz.

{% include toc %}

Aipatutako atala [hemen](/tdd-hastapena-fizz-buzz-kata-1) daukazue ikusgai baina laburpen moduan, *parse* funtzio bat sortu genuen zenbaki bat jasotzen duena eta ondokoa itzultzen duena: 3ren multiploa bada *Fizz*, 5en multiploa bada *Buzz*, 3 eta 5en multiploa bada *FizzBuzz* eta bestela, jasotako zenbakia bera.

Hau litzateke kodea.

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

Hou guztia TDD jarraituz egin genuan eta beraz test bidez babesturik dago kodea. Honi esker lasai birfaktorizatu dezakegu. Egia da kodea nahiko xinplea eta motza dela, baina badauka kode-bikoiztasunik. Agian ez da oso agerikoa baina zenbaki bat beste bategatik zatitu eta hondarra zero den ala ez begiratzen duen kode zatia behin eta berriz erabiltzen ari gara. Kasu honetan dena den, kode-bikoiztasunagatik baino gehiago kodearen ulergarritasunagatik, kode zati hori funtzio batetara aterako nuke.

Izan ere, kasu bakoitzean emaitza bat ala beste itzultzeko zein kondizio edo baldintza bete behar den ulertzea zaila ez den arren, denbora apur bat eskatzen du, eta beraz hau ulertzea azkarragoa eta errezagoa izan dadin egin badezakegu zergaitik ez egin?

Egia da bestalde gure testak ere badauzkagula eta dakigun bezela, testek, kodearen funtzionamendu zuzena eta eskakizunen betetzea ziurtatzeaz gain, dokumentazio eginkizuna ere badute.

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

    it("should return the number if not divisible by 3 nor 5", () => {
        const result = fizzBuzzer.parse(2);

        expect(result).to.be.equal("2");
    });
});
```

Testei begiratuta nahiko agerikoa da zein diren eskakizunak eta zein baldintza bete behar den kasu bakoitzean, baina horrek ez du kentzen gure kodea berez auto-esplikatiboa izan dadin. *parse* funtzioa irakurtzen duen edonork ez luke zertan testetara joan behar azalpen argiago bat jasotzeko. Hori denbora galtze bat da eta gainera gehiagotan esan bezala, garatzaileok denbora gehiago pasatzen dugu kodea irakurtzen idazten baino, beraz, saia gaitezen hau ahalik eta errezen irakur eta uler dadin.

Hori egiteko modu bat honako zerbait egitea litzateke.

```javascript
const numberDivisibleBy = (number, divisor) => number % divisor === 0

class FizzBuzzer {

    parse(number) {
        if (numberDivisibleBy(number, 3) && numberDivisibleBy(number, 5)) {
            return "FizzBuzz";
        }

        if (numberDivisibleBy(number, 3)) {
            return "Fizz";
        }

        if (numberDivisibleBy(number, 5)) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Funtzio bat sortu dugu zeinari zenbakia eta zatitzailea pasatzen dizkiogun eta hondarra zero den ala ez esaten digun. Kodea askoz hobe dago horrela, baina funtzioaren lehen parametroa, *number* alegia, erredundante samarra da. Kasu guztietan zatitzailea zenbaki berarekin aplikatu nahi dugu. Eta ez hori bakarrik, semantikoki ere irakurtzeko garaian ez da ahalko lukeen bezain "elegante" gelditzen.

Askoz naturalago irakurriko litzateke funztioak soilik zatitzailea jasoko balu eta honela *divisibleBy* horrek zatitzaileari egingo balio erreferentzia.

Bada JavaScriptek (eta beste lengoaia funtzional batzuk, edo denek beharbada) badauka gauza bat *Curry*fikazioa edo *Currying* esaten zaiona. *Currying*-a, parametro bat baino gehiago jasotzen duen funtzio bat hartu eta hau parametroak banan bana jasoko dituen beste batean bilakatzean datza.

Hau idatzi ordez.

```javascript
const sum = (firstNumber, secondNumber) => firstNumber + secondNumber;

const sumResult = sum(1, 4);
```
Hau idaztea.

```javascript
const sumTo = (firstNumber) => (secondNumber) => firstNumber + secondNumber;

const sumToOne = sumTo(1);
const sumResult = sumToOne(4);

```

Ikusten baduzue, hori lortzeko egin duguna da parametro bakoitzagatik funtzio bat sortu parametro bat jasotzen duena eta beste funtzio bat itzultzen duena. Beste funtzio honek hurrengo parametroa jasoko du. Azken funtzioak (kasu honetan bi soilik dira) hasierako kodea exekutatuko du.

Hau guk egin ordez, *lodash* bezelako liburutegi bat erabiliz egin genezake. Honek *curry* funtzio bat dauka zeinari gure hasierako funtzioa pasatzen badiogu, objektu bat itzuliko digun parametroak banan bana jaso ditzakena. Hau egitearen abantaila nagusiena honakoa da. *Currying* bidez deitu nahi dugun funtzioa gure kodean toki gehiagotan erabiltzen bada, arazoak eduki genitzazke berau deitzeko modua aldatuz gero. *Lodash*eko *curry* funtzioa erabilita, funtzio originala ez genuke aldatuko eta soilik guk nahi dugun tokian izango genuke gure funtzioaren bertsioa *curryfikatua*.

Nik dena den, aldez aurretik funtzio hori modu *curryfikatuan* deituko dudala baldin badakit, nahiago dut zuzenean modu horretan sortu eta ez kanpoko liburutegirik erabili.

Fizz Buzz katako adibidera itzulita, gure kodearen bertsio *curryfikatua* hau izango litzateke.

```javascript
const divisibleBy = (number) => (divisor) => number % divisor === 0

class FizzBuzzer {

    parse(number) {
        const numberDivisibleBy = divisibleBy(number);

        if (numberDivisibleBy(3) && numberDivisibleBy(5)) {
            return "FizzBuzz";
        }

        if (numberDivisibleBy(3)) {
            return "Fizz";
        }

        if (numberDivisibleBy(5)) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Ikus dezakezuen moduan, *numberDivisibleBy* funtzioa *divisibleBy* bezela izendatu dugu eta orain bi parametro batera jaso ordez, lehenik bat jasotzen du eta funtzio berri bat itzultzen du bigarren parametroa jasoko duena. Horrek ahalbidetzen du *divisibleBy* behin deitzea jaso dugun zenbakiarekin eta honekin funtzio berri bat sortzea zatitzailea soilik espero duena. Semantikoki askoz adierazgarriago da funtzio berri honek parametro bakarra jasotzen duenez hitz-jokoa egin dezakegulako eta `numberDivisibleBy(3)` modu nahiko naturalean irakurtzen delako.

Noski honelako emaitza bat lortzeko modu bakarra ez da *currying*-a erabiltzea. Funtzioak modu natural batean itzuli ezin ditzaketen lengoaietan adibidez klase bat sor genezake eraikitzailean zenbakia jasotzen duena eta ondoren zatitzailea jasoko duen metodo bat daukana.

```javascript
class NumberDivisibilityChecker {
    constructor (number) {
        this._number = number;
    }

    divisibleBy(divisor) {
        return this._number % divisor === 0;
    }
}

class FizzBuzzer {

    parse(number) {
        const checkNumber = new NumberDivisibilityChecker(number);

        if (checkNumber.divisibleBy(3) && checkNumber.divisibleBy(5)) {
            return "FizzBuzz";
        }

        if (checkNumber.divisibleBy(3)) {
            return "Fizz";
        }

        if (checkNumber.divisibleBy(5)) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Honela antzeko efektu bat lortzen dugu, baina egia da kode gehiago behar izan dugula eta bestalde, klase bat sortu behar izan dugunez, zuzenean funtzioa bat deitu ordez, klase bateko metodo bat deitzen ari gara, eta gure kasu zehatzean behintzat, exekuzio honekin hitz joko aprosopa lortzea pixka bat zailagoa izan daiteke. `checkNumber.divisibleBy(3)` osatu dugu ahalik eta naturale irakur dadin, baina baldintza gehitzen diogun momentuan `if (checkNumber.divisibleBy(3))` jada ez da berdin irakurtzen. Efektu bera lortzeko `NumberDivisibilityChecker` klasearen instantziari `number` deitu beharko genioke `if (number.divisibleBy(3))` irakurri ahal izateko, baina `number` parametroarekin arazoak izango genituzke.

Dena den, bai modu batean ala bai bestean, merezi du gure kodeari semantika gehigarri hori ematen saiatzea gure kodea irakurtzen atsegina izan dadin, horrek seguruenik esan nahiko bait du aldi berean irakurtzen, eta batez ere ulertzen, erreza izango dela.
