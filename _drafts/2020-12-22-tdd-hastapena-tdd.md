---
layout: post
title: "TDD hastapena: TDD"
categories:
  - TDD
tags:
  - TDD
  - Test
  - Programazioa
---

TDD hastapenen inguruko post-multzoko bigarren atala da honakoa. [TDD hastapena: Testak](/tdd-hastapena-testak/) postaren jarraipena da. Oraingo hontan zuzenean TDD berari helduko diogu, ezaugarri nagusienak, bere onurak eta zenbait kode adibide ikusiz.

{% include toc %}

# ZER DA TDD?

TDD ingelesezko *Test Driven Development*-en akronimoa da eta bere esanahia euskaraz Testek Gidaturiko Garapena edo litzateke. TDD softwarea garatzeko prozesu edo teknika ebolutibo bat bezela deskribatu daiteke, non testak produkzioko kodea baino lehenago idaztea eta birfaktorizazioa konbinatzen diren.

Kent Beck AEBetako software-ingeniari egozten zaio berau asmatu edo "berraurkitu" izana 2003 urtean. XP edo eXtrem Programming metodologiaren parteetako bat da, beharbada, hedatuena.

# ZERGATIK TDD?

[Aurreko](/tdd-hastapena-testak/) atalean ikusi genuen moduan, gure softwarea amaitutzat emadeko eta guztia ongi dagoela frogatzeko ez da nahikoa gure aplikazioa hartu eta erabiltzaile batek egingo lukeen moduan erabiltzea. Argi dago ez ditugula kasu guztiak frogatuko, eta egiten saiatuko bagina, daukaguna baino denbora gehiago beharko genuke ziurrenik.

Hau ekiditeko ordea, ikusi genuen Test Automatikoak erabil ditzakegula, beraz, zertarako behar dugu TDD?

Lehenik eta behin, esan behar da TDD Test Automatikoetan oinarritzen dela, eta beraz, ez dator hauek ordezkatzera, hauek modu zehatz batean erabiltzeko proposatzera baizik, horrekin, besteak beste, gure kodearen kalitatea eta kode-estaldura hobe bat bermatzeko.

# TDD-REN 3 LEGEAK

Robert C. Martin-ek (Uncle Bob bezela ere ezagutzen denak) honako 3 arauak erabiltzen ditu TDD deskribatzeko:

> - You are not allowed to write any production code unless it is to make a failing unit test pass.
> - You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
> - You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

- Ezin da prokuzio koderik idatzi baldin eta fallatzen duen test unitario bat pasa arazteko ez bada.
- Ezin da fallatzeko nahikoa izango den test unitario bat baino gehiago idatzi; eta konpilazio erroreak fallotzat hartzen dira.
- Fallatzen dagoen testa pasa arazteko beharrezkoa den gutxieneko produkzio kodea baino ezin da idatzi.

Aitortu behar det, bigarren legea erredundante samarra iruditu zaidala beti, eta horregatik, nahiago det [hemen](http://www.javiersaldana.com/articles/tech/refactoring-the-three-laws-of-tdd) proposatzen den bertsio murriztua:

> - Write only enough of a unit test to fail.
> - Write only enough production code to make the failing unit test pass.

- Idatzi soilik fallatzeko nahikoa den test unitario bat.
- Idatzi soilik fallatzen ari den testa pasa arazteko nahikoa den produkzio kodea.

Hiru lege hauek ematen diote TDDri bere balio guztia. Hauek ezberdintzen dute TDD soilik Test Unitarioak idaztetik. Testak produkzio kodea baino lehen idatziko bagenitu ere, lege hauek bete ezean, ez ginateke TDD egiten ariko.

Dena den, hiru lege hauek TDDren arauak deskribatzen badituzte ere, bada ezagutu beharreko beste kontzeptu bat, izan ere, TDD batez ere beraugatik ezanguna da, eta hau *Red Green Refactor* da.

# TDD ZIKLOA (RED GREEN REFACTOR)

TTD-ren zikloaz hitz egiten denean **Red Green Refactor** erabili ohi da askotan, hauek direlako nolabait zikloa osatzen duten faseak.

Lehen pausoa, ondoko diagraman ikus daitekeen moduan, eta aurretik aipatutako 3 legeetan oinarrituz, fallatzen dugun test bat (eta bakarra) edukitzea da. Orokorrean test liburutegietan fallatzen duten testak gorriz markatu ohi dira eta horregaitik lehen pauso hau *red*-ekin identifikatzen da.

Bigarren pausoa test hori pasa araztea da, hori bai, gogoratu, beharrezkoa den gutxieneko produkzio kodea erabiliz soilik. Errorerik ematen ez duten testak, hau da, pasatzen dutenak, berdez markatu ohi dira, eta hortik pauso honi *green* deitzea.

Hirugarren eta azken pausoa, kodea birfaktorizatzea litzateke, beti ere gure test guztiak berdean daudelarik eta soilik beharrezkoa bada, redundantzia edo kode errepikapenen bat ezabatzeko, aldagai edo objekturen bat berrizendatzeko, edo kodea ulergarriagoa egin dezakeen edozein aldaketa burutzeko.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/tdd-cycle.png' | absolute_url }}" alt=""></a>
  <figcaption>TDD-ren zikloa</figcaption>
</figure>

Ziklo hau behin eta berriz hasiko genuke test berri bat gehitzen dugun bakoitzean, gure eskakizun guztiekin amaitu arte.

# GREEN BAR PATTERN

TDD egiten ari garenean eta gorrian dagoen test bat daukagunean, helburua beharrezkoa den gutxieneko produkzio kodea erabiliz hau berdean jartzea da, baina ez hori bakarrik, testa berdean ahalik eta azkarren jartzen saiatu behar dugu ere. 

Hau lortzeko, aukera ezberdinak daude eta horiek *Green Bar Pattern* deiturikoaren barnean izendatu zituen Kent Beck-ek bere *Test Driven Development By Example* liburuan.

## FAKE IT (TILL YOU MAKE IT)

Test bat berdean jartzeko modurik azkarrena espero dugun balorea zuzenean itzultzea litzateke, konstante moduan. Adibide batekin ikusiko dugu. Demagun, emandako urte bat bisiestoa den ala ez ebazteko aplikazioa bat garatu nahi dugula eta esan digute, urte bat bisiestoa izateko baldintza hauek bete behar dituela.

> Urte bat bisiestoa da baldin eta:
> - 4-ren multiploa den
> - Ez den 100-en multiploa
> - 100-en multiploa bada 400-ena ere baden

Hau horrela izanik, test hau idatz genezake.

```javascript
describe('LeapYearChecker', () => {
    it('should determine that is not a leap year if not divisible by 4', () => {
        const leapYearChecker = new LeapYearChecker();
        expect(leapYearChecker.isLeap(2001)).to.equal(false);
    });
});
```

Eta berau pasa arazteko modurik azkarrena honakoa litzateke.

```javascript
class LeapYearChecker {
    isLeap() {
        return false;
    }
}
```

Hau egiteari *fake implementation* edo inplementazio faltsua esaten zaio. Helburua testa berdean ahalik eta azkarren jartzea zen eta hori lortu dugu. Badakigu kode horrek ez dituela gure eskakizuneko kasu guztiak beteko, baina bat behintzat bai, eta izenburuak dioen bezela (*till you make it*), iritsiko da unea nun kode hau orokortuko dugun.

Momentuz, gure testak berdean daude, eta beraz, test gehiago idazteko moduan gaude.

## TRIANGULATE

Triangelatzea implementazioa faltsuaren hurrengo pausoa litzateke, honek soluzio orokorrago batera gidatu gaitzan. Triangelatzen hasteko, gutxienez bi kasu ezberdin eduki behar ditugu metodo beraren gainean.

Aurreko adibidearekin jarraituz, helburua, edozein urte ematen diogula ere, hau bisiestoa den ala ez esango digun funtzio bat lortzea da, beraz, kasu gehiago idatzi behar ditugu.

```javascript
it('should determine that is a leap year if divisible by 4', () => {
    const leapYearChecker = new LeapYearChecker();
    expect(leapYearChecker.isLeap(1996)).to.equal(true);
});
```

Test honek noski fallatu egingo du, eta pasatzeko modu azkar bat hau egitea litzateke.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year !== 2001) {
            return false;
        }
        return true;
    }
}
```

Kasu batzuetan bide hau jarraitzeak zentzua dauka, behar bada, ez daukazunean argiegi ze baldintzak batzen dituen bi kasu ezberdin edo zein bide jarraitu oso argi ez daukazunean. Hau egitea ***baby step*** kontzeptua muturreraino aplikatzea litzateke, eta kasu honetan, lehenengo testaren gainean (hots, lauren multiploa **ez** izatea egiaztatzen duena) bigarren kasu bat gehitzeak behartuko liguke kodea gehiago orokortzera.

```javascript
describe('LeapYearChecker', () => {
    it('should determine that is not a leap year if not divisible by 4', () => {
        const leapYearChecker = new LeapYearChecker();
        expect(leapYearChecker.isLeap(2001)).to.equal(false);
        expect(leapYearChecker.isLeap(1997)).to.equal(false);
    });
});
```

Honek nolabait behartuko liguke edo bigarren erabaki adar bat jartzera.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year !== 2001) {
            return false;
        }
        if (year !== 1997) {
            return false;
        }
        return true;
    }
}
```

Edo zuzenan, bi kasuek komunean zer daukaten begiratzera. Gure kasuan argi daukagu lehen adibidea zergaitik aukeratu dugun, eta hain zuzen lauren multiploa **ez** delako da, beraz, hau jakinda, zentzu gehiago izango luke zuzenean baldintza hori idazteak.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year % 4 !== 0) {
            return false;
        }
        return true;
    }
}
```

Hirugarren test bat idatziko dugu jarraian, hurrengo baldintza bete dadin.

```javascript
it('should determine that is not a leap year if divisible by 100', () => {
    const leapYearChecker = new LeapYearChecker();
    expect(leapYearChecker.isLeap(1900)).to.equal(false);
});
```

Ta dagokion kodea testa berdean lehen bait lehen jartzeko.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year % 4 !== 0) {
            return false;
        }
        if (year % 100 === 0) {
            return false;
        }
        return true;
    }
}
```

Bistakoa da, bi kondizioak bakar batean batu litezkeela, bi kasutan *false* itzuliko bait dugu, baina ariketa ona izan daiteke honela idaztea, azken finean, gure testak dio 100-en multiploa bada ez dela bisiestoa, ta beraz, oraindik pauso txikiagoa da beste *if* bat gehitzea. Behin testa berdean dagoela, TDDren hirugarren pausoa baliatuz, hau birfaktoriza genezake eta honela utzi.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year % 4 !== 0 || year % 100 === 0) {
            return false;
        }
        return true;
    }
}
```

Ikusten duzuen moduan, testak gero eta zehatzagoak izan, gure kodea gero eta orokorragoa da. Azken baldintza gehituko dugu amaitzeko.

```javascript
it('should determine that is a leap year if divisible by 100 but also by 400', () => {
    expect(leapYearChecker.isLeap(2000)).to.equal(true);
});
```

Ta gorrian dagoenez, berdean jartzeko beharrezko kodea idatziko dugu. Hau ere, bi pausotan egin dezakegu, errezagoa izan dadin. Hori bai, ordena garrantzitsua da, eta kasu honetan 400, 100en multiploa denez, hasieran jarri behar dugu gure baldintza exekutatu dadin.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year % 400 === 0) {
            return true;
        }
        if (year % 4 !== 0 || year % 100 === 0) {
            return false;
        }
        return true;
    }
}
```

Ta behin berdean dagoela, aldaketa egin dezakegu baldintza *if* berdinean sartzeko.

```javascript
class LeapYearChecker {
    isLeap(year) {
        if (year % 4 !== 0 || (year % 100 === 0 && year % 400 !== 0)) {
            return false;
        }
        return true;
    }
}
```

Honekin, gure funtzionalitatea bukaturik legoke. Baldintza guziak betetzen direla egiaztatu dugu gure testen bidez. Horrek esan nahi du amaiturik dagoela? ez du zertan. TDDren hirugarren pausoak esaten digu behar izanez gero, birfaktorizatzeko. Albiste ona, testak dauzkagunez, lasai egin dezakegula da. Adibide gisara, aurreko kodea, beste hontan bihur genezake.

```javascript
class LeapYearChecker {
    isLeap(year) {
        return this._isDivisibleBy(4, year) && 
            (!this._isDivisibleBy(100, year) || this._isDivisibleBy(400, year));
    }

    _isDivisibleBy(divisor, year) {
        return year % divisor === 0;
    }
}
```

Norberak erabakiko du ulergarriagoa eta mantengarriagoa den ala ez. Alde batetik kondizioari buelta eman diot, bisiestoa noiz **ez** den egiaztatu ordez alderantzizkoa egin dezan, normalean errezagoa bait da baiezkoa interpretatzea. Bestetik, zatiketaren hondarra 0 den egiaztatzea funtzio batera atera det, eta azkenik, dena lerro batean idatzu dut. Aldaketa guzti hauek testak ikutu gabe egin ditut, eta hori da gakoa. Azken finean, birfaktorizatzea, kodearen **portaera aldatu gabe**, kodean aldaketak egitea da. Bide batez, jakin kodea, pausoz pauso, eskuragarri daukazuela [github](https://github.com/ibaiimaz/leap-year-kata)-en.

Bukatzeko soilik esan, triangulazioak funtziona dezan, gehitzen dugun test bakoitzak eskakizun bat betetzeaz gain noski, gure produkzio kodean, zuzena ez den edo oraindik nahikoa orokorra ez den atal bat aldatzera behartu gaitzan saiatu behar garela.

## OBVIOUS IMPLEMENTATION

Badaude kasuak non burutu beharreko ataza nola ebatzi argi daukagun. Kalkulagailu bat garatzen ari bagara eta bi zenbaki batzeko gai den funtzio bat idatzi nahi badugu, ez dauka zentzurik ez inplementazio faltsua, ezta triangelazioa erabiltzen ibiltzeak, zuzenean batuketa idaztea litzateke egokiena. Azken finean, triangelazioa (eta baita inplementazio faltsua), burutu beharreko algoritmoa argi ez daukagunean dira erabilgarri.

Hori bai, kontuz ibili, askotan garatzaileok arinegi ibiltzen bait gara problema baten ebazpenaren zailtasuna aztertzerakoan, eta jada nahaste borraste izugarria daukagunean ohartzen gara ez zela zirudien bezain begibistakoa eta berriro hasi behar gara pausu txikiagoak emanez.

# BABY STEPS

Aurreko adibidean ikusitako moduan pauso txikiak emanez kodea idazteari, definitiboa izango ez den tarteko kodea idatziz, pixkanaka test berri bakoitzak soluzio finalera gidatu bitartean, *baby steps* deitzen zaie.

Baby Steps bidezko TDDa Triangelazioarekin konbinatzeak, ebatzi beharreko problemaren konplexotasun perzepzioa hobetzen laguntzen du, izan ere, erabaki bakoitza ia bistakoa da hartzen, eta berau hartzeko eta aurrera eramateko egin beharreko esfortzua erabat txikia da.

Egia da hartu beharreko erabaki kopurua areagotu egiten dela, baina aldi berean, hauetako bakoitzaren konplexotasuna gutxitu egiten da. Gainera, aldaketa txiki bakoitzaren ostean, testak exekutatuko ditugu ezer hautsi ez dela egiaztatzeko, eta gertatuko balitz, aztertu beharrekoa kode kopurua hain da txikia, erreza dela akatsa aurkitzen, edo besterik gabe, azken aldaketak desegiten.

# NOLA AUKERATU ZE TEST IDATZI

Zein test idatzi erabakitzea zaila izaten da batzuetan. Izan ere, testen ordena garrantzitsua da orokorrean. Lagungarria izaten da bete behar ditugun eskakizunen definizioa hartu eta hauek betetzeko bururatzen zaizkigun test kasuak zein izango diren zerrendatzea. Ez ditugu ere kasu guztiak aldi batez pentsatu behar, testak idazten goazen arabera, betetakoak ezabatzen joango gara eta bururatzen zaikigun berriak idazten.

Behin hau eginda, adibide xinpleenetik hasten saiatu behar ginateke. Kontuan izan test bakoitzak portaera edo baldintza bat frogatu behar duela, ez portaera edo baldintza berdinaren adibide ezberdin bat. Aurreko adibidera itzuliz, urte bat lauren multiploa den ala ez frogatzerakoan, gure kodea orokortzeko, kasu bat baino gehiago beharko bagenitu, nire aholkua test berdinaren gainean egitea da. Nire ustetan ondokoa litzateke modu zuzena.

```javascript
describe('LeapYearChecker', () => {
    it('should determine that is not a leap year if not divisible by 4', () => {
        const leapYearChecker = new LeapYearChecker();
        expect(leapYearChecker.isLeap(2001)).to.equal(false);
        expect(leapYearChecker.isLeap(1997)).to.equal(false);
    });
});
```
Bi **expect** idatzi nahi ez baditugu, test liburutegiaren arabera, badago aukera testak parametrizatzeko, jartzen ditugun kasuak bezainbeste aldiz exekutatu dadin. Nik aukeran, testaren konplexutasunak ahalbidetzen badu, nahiago dut goiko aukera, baina bai bata zein bestea, beti izango dira hobe ondorengo adibidea baino.

```javascript
describe('LeapYearChecker', () => {
    it('should determine that is not a leap year when year is 2001', () => {
        const leapYearChecker = new LeapYearChecker();
        expect(leapYearChecker.isLeap(2001)).to.equal(false);
    });

    it('should determine that is not a leap year when year is 1997', () => {
        const leapYearChecker = new LeapYearChecker();
        expect(leapYearChecker.isLeap(1997)).to.equal(false);
    });
});
```

Goiko kasuan, testaren textua irakurriz, ez da batere argi gelditzen bi **adibideek** komunean daukatena dela **ez** direla 4-ren multiplo. Bi kasuetan baldintza berdina da eta beraz, nire ustetan behintzat, test berdina behar luke izan.

Bestalde, saiatu behar gara test bakoitzak, alde batetik oraindik bete gabe dagoen baldintza bat froga dezan, ta bestetik, kodea orokortzea bultza dezan.

Gerta liteke ere, test berri bat idatzi eta hau zuzenean berdean egotea. Horrek bi azalpen izan ditzake. Batetik, adibide hori jada gure implementazioan jasorik dagoela, eta behar bada ez daukagula bete gabeko kasurik eta gure kodea amaiturik dagoela. Ta bestetik, izan daiteke momentu honetan, kodeak duazkan baldintza jakinengatik ez fallatzea, baina aurrerago egitea. Hau bada kasua, esan nahi du ez dugula test aproposa aukeratu eta beraz, utzi aurreragorako test hori. Ahal dela, ez idatzi fallatuko ez duen testik.

Bide batez esan, hasiera batean, Baby Step-ak jarraitzearren, test bat idatzi badugu baina ondoren, beste test batek, kasu hori ere bere baitan hartzen badu, zuzena dela test erredundante hori ezabatzea.

# TESTAK IZENDATZEN

Gure kodearen edozein ataletan bezela, testetan ere ohiko praktika on guztiak aplikatzea komeni da eta horien barnean sartzen da izenen kontua. Testak izendatzerakoan, saiatu behar gara hauek ahalik eta deskribatzaileenak izatea. Azken finean, testen abantaileko bat, dokumentazio moduan balio dezaketela da.

Gorago aipatu dugu test bakoitzak portaera edo baldintza bat frogatu behar duela eta beraz, bere izanek hori isladatu behar du, eta ez frogarako erabiltzen ari garen kasu zehatza.

Bestalde, rSpec familiako liburutegiek, beren idazteko moduagatik, aukera ematen dute modu nahiko naturalean idazteko testaren baldintza. Bestalde, lagungarria izaten da testaren hasieran *should* hitza idaztea (beti ere testak ingelesez idazten badituzue), honek izen adierazgarriagoak jartzera bultzatuko bait gaitu. Hau kontutan harturik, ondokoa izen desegoki bat litzateke.

```javascript
describe('SuperCalculator' () => {
    it('returns 9 when 3 passed' () => {
        const calculator = new SuperCalculator();
        expect(calculator.squareOf(3)).to.equal(9);
        ....
```

Beste hau berriz askoz aproposagoa litzateke.

```javascript
describe('SuperCalculator' () => {
    it('should calculate the esquare of a given number' () => {
        const calculator = new SuperCalculator();
        expect(calculator.squareOf(3)).to.equal(9);
        ....
```

Esan bezela, test batean kasu zehatz bat frogatzen dugun arren, testaren izenak ahalik eta orokorrena izan behar du.

# BI TXAPELEN METAFORA

Mi txapelen metafora Martin Fowlerrek izendatu zuen programatzaileok gure lana egiten ari garenean "jantzi" beharko genituzkeen bi txapel (edo bi rolak) aipatzeko.

Honen arabera, programatzaileok bi txapel ezberdin "jantzi" behar ditugu, bata portaera aldaketak egiten ari garenean, hau da, funtzionalidade berriak gehitzen ari garenean (testak idaztea honen barruan sartuko litzateke ere), eta bestea, aldaketa estrukturalak egiten ari garenean, portaera aldatu gabe, hau da, birfaktorizatzen. Bi txapel hauek ezin dira inoiz batera jantzi, aurrena bat jantzi beharra dago eta gero bestea. Honek, une bakoitzean betebehar zuzenean zentratzen laguntzen gaitu.

Txapela bat besteagatik maiz aldatuz eraiki beharko genuke softwarea, funtzionalidade oso txikiak garatuz eta testatuz, jarraian hauek birfaktorizatu eta kodearen diseinua eta kalitatea hobetzeko. Honela, portaera aldaketak *Red* eta *Green* faseetan zehar egingo genituzke, eta aldaketa estrukturalak Birfaktorizazio fasean zehar.



# TDD-REN ONURAK

TDD erabiltzeak onura ezberdinak dauzka. Egia da, guztiak ez direla TDDrenak propio, batzuk test automatikoak idaztearen ondorio dira, ordena dena delakoa izanda ere, baina nire gomendioa, testak modu intentsibo batean erabiliko badituzu, zuzenean TDD erabiltzea da, beraz denak saku berean sartuko ditut.

Ageriko onuretako bat da test estaldura oso haundia lortuko dugula TDD egiterakoan. Kodea idatzi aurretik testa idazten dugunez, normalena gure kodearen zati haundiena testez estalia egotea litzateke. Egia da, dena den, test estaldura haundia izatea ez dela kodel kalitate onaren bermea, ta aldi berean, ez da helburu bat bere baitan, TDDren ondorio bat bezala ikusten dut gehiago nik, hori bai, honek beste albo onura batzuk dauzka.

Esaterako, ***Boy Scout*** delakoaren araua aplikatu ahal izatea sustatzen du. Honek dio, geure kodearen gainean lanean ari garen bitartean, hobetu litekeen edozein atal ikusiz gero, gure ardura dela berau hobetzea. Hori bai, hau portaera berri bat garatzen ari garen bitartean ikusiko bagenu, gomendagarriena, nunbaiten apuntatu eta portaeraren garapena bukatutakoan burutzea litzateke, bi zereginak bata besteagandik bananduz.

Beste onura nagusia gure aplikazioa diseinatzen laguntzen digula da. Are gehiago, batzuk *Test Driven Development*-eko *Development*, *Design*-gaitik aldatzea proposatzen dute. Kontua da, testak aurretik idazteak laguntzen duela nolabiat gure aplikazioaren *interfazea* definitzen, eta aldi berean, kodea testek eskatutakoa betetzera soilik mugatzen denez, diseinu sinpleagoak lortzen ditugu eta ***YAGNI*** deiturikoa ekiditen laguntzen digu.

*YAGNI* ingelesezko *You ain't gonna need it*-etik dator, eta *hori ez duzu beharko* esatera dator. Izan ere, programatzaileok joera daukagu zenbaitetan etorkizunean izango ditugun beharrei begira garatzeko, eta beraz, unean behar ez arren, zenbait funtzionalitate garaturik uztea edo gure kodea hauetan pentsatuz diseainatzea, eta askotan, hori behar izango dugun eguna sekula ez da iristen.

Hori bai, ez dezagun nahastu hau *SOLID*-eko *Open-Close* printzipioarekin. Gauza bat da, gure kodea hedatzen erreza izatea, ta beste bat gure kodea etorkizunean irits litekeen eskakizun batetan pentsatuz diseinaturik egotea.

# ONDORIOAK

TDD teknika oso interesgarria da, goian aipatutako onurengaitik eta aipatu gabe utziko nituen beste askorengatik ere, eta hasiera batean berau menperatzeak denbora apur bat eskatzen duen arren, epe erdi luzera, denbora inbertsio hori segituan amortizatzen da. Denok dakigu softwarea gauza bizia dela eta ohikoa dela berau aldatzea eta hedatzea. Une hori iristean, ziurrenik TDD bidez diseinatu izanak aplikazioa hedatzea erreztuko dizu, eta bestetik, test estaldura zabala izateak, ordaindu ezin daitekeen lasaitasun bat emango dizu.
