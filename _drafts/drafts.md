# Bonusak

## "*Curry*fikazio" birfaktorizazioa

Kodea testen bidez babesturik daukagunez lasai birfaktorizatu dezakegu. Egia da kodea nahiko xinplea eta motza dela, baina badauka kode-bikoiztasunik. Agian ez da oso agerikoa baina zenbaki bat beste bategatik zatitu eta hondarra zero den ala ez begiratzen duen kode zatia behin eta berriz erabiltzen ari gara. Kasu honetan dena den, kode-bikoiztasunagatik baino gehiago, kodearen ulergarritasunagatik kode zati hori funtzio batetara aterako nuke.

Hori egiteko modu errezena honako zerbait egitea litzateke.

```javascript
const divisibleBy = (number, divisor) => number % divisor === 0

class FizzBuzzer {

    parse(number) {
        if (divisibleBy(number, 3) && divisibleBy(number, 5)) {
            return "FizzBuzz";
        }

        if (divisibleBy(number, 3)) {
            return "Fizz";
        }

        if (divisibleBy(number, 5)) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Funtzio bat sortu dugu zeinari zenbakia eta zatitzailea pasatzen dizkiogun eta hondarra zero den ala ez esaten digun. Kodea askoz hobe dago horrela, baina funtzioaren lehen parametroa, *number* alegia, erredundante samarra da. Kasu guztietan zatitzailea zenbaki berarekin frogatu nahi dugu. Bada JavaScriptek (eta beste lengoaia funtzional batzuk, edo denek beharbada) badaukate gauza bat *Curry*fikazioa (*Currying* inglesez) esaten zaiona. Beste blog sarrera batean akaso saiatuko naiz sakonago azaltzen zer den, baina laburbilduz, gure kasuan egingo duguna da bi parametro jasotzen dituen gure funtzioa, parametro bakar bat jasota, parametro bat jasotzen duen funtzio bat itzultzen duen funztio batetan bilakatu. Horrela azalduta ez da ondoegi ulertzen beraz onena kodea erakustea izango da.

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

Ikus dezakezuen moduan, *divisibleBy* funtzioak orain bi parametro batera jaso ordez, lehenik bat jasotzen du eta funtzio berri bat itzultzen du, bigarren parametroa jasoko duena. Horrek ahalbidetzen du *divisibleBy* behin deitzea jaso dugun zenbakiarekin, eta honekin funtzio berri bat sortzea zatitzailea espero duena. Semantikoki ere adierazgarriago da funtzio berri honek parametro bakarr jasotzen duenez hitz-jokoa egin dezakegulako `numberDivisibleBy(3)` modu nahiko naturalean irakurtzen delako. Pintzeladatxo bat baino ez da baina interesgarria iruditzen zitzaidan aukera hau aipatzea.

## Eskakizun bakoitzerako test kasu bat baino gehiago

Ni beti saiatzen naiz nire testak dokumentazio bezala balio dezaten eta beraz, eskakizun bakoitza test batetan isladatzen saiatzen naiz. Bestalde uste dut test baten mezuak eskakizuna modu orokorrean deskribatu behar duela eta ez kasu konkretau bat. Hau da, `should return Fizz is divisible by 3` erabiltzen dut `should return Fizz when 6` idatzi beharrean (asumituz gure testean adibide moduan 6 zenbakia pasatzen ari garela).

Honek daukan alde "txarra" da testa kasu bakar batekin frogatzen dugula. Goiko bigarren kasuan bezela `should return Fizz when 6` jarriko bagenu, ondoren bigarren test bat idatz genezake `should return Fizz when 12` esaten duena, eta bi kasutan 3gatik zatigarria denean *Fizz* itzultzen duela frogatzen ariko ginateke.

Zergatik diodan hau? ba gerta litekeelako testa pasatzeko gutxieneko kodea idazterakoan, *baby steps* kontzeptua muturrerano eramatea eta kodea orokortuz joan beharrean honakoa idaztea.

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

        if (number === 10) {
            return "Buzz";
        }

        return number.toString();
    }
}
```

Ikusten baduzue, *Buzz* itzultzeko baldintza aldatu dut eta zuzenean 10 zenbakiaren berdina den ala ez begiratzen dut. Hau egin ezkero, testak berdin berdin pasatzen jarraitzen dute, gure testean *Buzz* kasurako pasatzen ari garen balioa 10 delako eta beraz, gure test zehatzaren kasurako baldintza horrek ere balio digulako. Baina funtzio honi 5en multiploa den beste edozein zenbaki pasako bagenio, ez liguke *Buzz* itzuliko.

Esaterako 20 pasatuz gero.

```javascript
it("should return Buzz if divisible by 5", () => {
    const result = fizzBuzzer.parse(20);

    expect(result).to.be.equal("Buzz");
});
```

Testak fallatu egiten du.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/buzz-fixed-value.png' | absolute_url }}" alt=""></a>
</figure>

Beraz batzuetan test baterako kasu bat baino gehiago exekutatzeak lagun gaitzake kasu ezberdinak betetzen ditugula ziurtatzen. Hau egin nahiko bagenu, nahiz ta ni orokorrean ez naizen hau egitearen aldekoa, modu ezberdinak ditugu. Test liburutegi askok test parametrizatuak exekutatzeko aukera daukate, hau da, test bakar bat idatzi baina konfigurazio bidez exekutatzeko balio bat baino gehiago pasa.

*Java* lengoaierako *JUnit* liburutegiaren kasuan estareko horrela izango litzateke.

```java
@ParameterizedTest
@ValueSource(ints = {1, 3, 5, -3, 15})
void isOdd_ShouldReturnTrueForOddNumbers(int number) {
    assertTrue(Numbers.isOdd(number));
}
```

*Mocha*ren kasuan ez dakar horrelakorik modu natiboan nahiz ta existitzen diren horrelakoak egiteko instalatu daitezkeen pakete ezberdinak. Dena den ez da zaila guk geuk kode bidez egitea.

```javascript
[5, 10, 20].forEach(number => {
    it("should return Buzz if divisible by 5", () => {
        const result = fizzBuzzer.parse(number);

        expect(result).to.be.equal("Buzz");
    });
});
```

Honekin esaterako test berdina hiru balio ezberdinekin exekutatzen ari gara. Honen alde txarra da testak exekutatzerakoan ikusten dugun emaita nahasi samarra gelditzen dela.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/multiple-test-values-1.png' | absolute_url }}" alt=""></a>
</figure>

Azken finean, behin bakarrik idatzi badugu ere, testa hiru aldiz exekutatzen ari gara, hiru aldiz idatzi bagenu bezela. Fallatuz gero, ez litzateke guztiz argi geratuko hiruetako zeinek fallatu duen.

Hau ekiditeko zenbaitzuk egiten dutena honakoa da.

```javascript
[5, 10, 20].forEach(number => {
    it(`should return Buzz when ${number}`, () => {
        const result = fizzBuzzer.parse(number);

        expect(result).to.be.equal("Buzz");
    });
});
```

Eta testetan jasoko genukeen emaitza beste hau da.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/multiple-test-values-2.png' | absolute_url }}" alt=""></a>
</figure>

Orain argi gelditzen da test exekuzio bakoitza zein kasu den, baina lehen esaten genuena, testak, nire ustez behintzat, eskakizuna zein den deskribatu behar du eta ez frogatzen ari garen kasu zehatza.

Kasu bakoitzean test liburutegiak ematen dizkigun aukerak baliatuz, horrelako zerbait egin genezake.

```javascript
context("should return Fizz if divisible by 5", () => {
    [5, 10, 20].forEach(number => {
        it(`${number} is divisible by 5`, () => {
            const result = fizzBuzzer.parse(number);

            expect(result).to.be.equal("Buzz");
        });
    });
});
```

`context` hitzak agrupazio maila bat gehitzeko aukera ematen digu, besterik ez, eta hau erabil genezake gure adibide ezberdinak elkartzeko eta gure testak semantikoagoak gelditzeko.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/fizzbuzz/multiple-test-values-3.png' | absolute_url }}" alt=""></a>
</figure>
z
Ni ez naiz hau egitearen oso lagun, baina aukera bat da ta ongi dago jakitea. Egia esan formula magikorik ez daukat kasu hauetarako. Nik ziurrenik, hasierako kasuan geneukan bezela utziko nuke, kasu bakar batekin besterik gabe, arreta jarriz gure kodea nahiko orokorra dela ziurtatzeko, eta kasu bat baino gehiago frogatu nahiko banu, kasu zehatz hontan, kontuan izanik testa exekutatzeko beharrezkoa kodea oso gutxi dela, ziurrenik besterik gabe bi edo hiru aldiz deituko nuke funtziora kasu ezberdinekin.

```javascript
it("should return Buzz if divisible by 5", () => {
    const result = fizzBuzzer.parse(5);
    expect(result).to.be.equal("Buzz");

    const result = fizzBuzzer.parse(10);
    expect(result).to.be.equal("Buzz");

    const result = fizzBuzzer.parse(20);
    expect(result).to.be.equal("Buzz");
});
```

Eta testak fallatuz gero, arduratuko nintzateke begiratzeaz zein lerrotan eman duen errorea hiru kasuetako zeinek fallatu duen argitzeko.

Baina esan bezala, *FizzBuzz* katan test bakoitzerako kasu bat frogatuko nuke soilik.