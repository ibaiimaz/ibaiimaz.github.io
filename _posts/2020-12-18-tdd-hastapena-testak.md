---
layout: post
title: "TDD hastapena: Testak"
categories:
  - TDD
tags:
  - TDD
  - Test
  - Programazioa
---

Honekin post-multzo bat hasiko dut *TDD* hastapenen inguruan. Asmoa, hasteko teoria pixka bat azaltzen duten zenbait post egitea da, azkenik bideoan grabatuko dudan *Kata* batekin amaitzeko.

{% include toc %}

Lehen galdera litzateke, zer dira testak?

Garatu dugun edo garatzen ari garen sistemak bete behar dituen eskakizunak betetzen dituela egiaztatzeko teknika multzoari esaten zaio *testing*-a Softwarearen esparruan.

Test hauek mota ezberdinetakoak izan daitezke. Lehen sailkapen bat hauek exekutatzeko moduaren arabera egin dezakegu, hau da, eskuz edo modu automatikoan exekutatzen diren aztertuz.

# ESKUZKO TESTAK

Eskuzko testing-a kasu batzuk prestatu eta beharrezko elementuak eskuz exekutatzean datza. Test hauek noski muga asko dauzkate. Eskuz egiten direnez motelak dira, gainera kasu ezberdinak prestatu behar ditugu baina hauek behin baino gehiagotan exekutatu ahal izateko erreplikatzen zailak dira, beraz, aldi berean garestiak direla esan daiteke. Gainera ez dira oso fidagarriak, kasuak ahaztu bait ditzakegu, eta batez ere, kasu guztiak betetzen oso zaila bait da.

Noski, kasu batzuetan beharrazkoak eta lagungarriak dira, baina zuzenena, kasu zehatz horietaz QA taldea arduratzea da. 

# TEST AUTOMATIKOAK

Test automatiko bat software zati txiki bat da, normalean funtzio bat, beste software unitate bat exekutatzen duena eta espero dugun emaitza edo efektua sortzen den egiaztatzen duena. Hau guztia egin ahal izateko liburutegi ezberdinak existitzen dira lengoaia eta teknologiaren arabera.

Test automatikoen artean ere mota ezberdinak bereiz ditzakegu.

## TEST EZ-FUNTZIONALAK

Test ez-funtzionalen helburua da sistema batek nola funtzionatzen duen epaitzeko erabil daitezkeen irizpideak zehazten dituen baldintza bat egiaztatzea, hala nola erabilgarritasuna, irisgarritasuna, mantengarritasuna, segurtasuna eta/edo errendimendua. Hau da, sistemak nola erantzuten duen egiaztatzen dute.

Mota ezberdinetako testak aurkitu ditzakegu multzo honetan baina ez dugu hauetan sakonduko. Dena den, aipatzearren, honakoak aurki genitzazke: Abiatura-testak, Karga-testak, Erabilgarritasun-testak, Segurtasun-testak, etab...

## TEST FUNTZIONALAK

Test funtzionalek gure aplikazioaren zati batek *negozioko* jendeak esandako eskakizunak betetzen dituela, eta gainea errorerik ez duela egiaztatzen dute. Test funtzionalen artean mota ezberdinetakoak daude, baina gu *Testen Piramidean* sartu ohi direnak aztertuko ditugu.

### TESTEN PIRAMIDEA

Test automatikoen piramideak, gure kode-basearen test-suitean zein test mota eta ze maiztasunekin azaldu beharko luketen adierazten du. Helburua garatzaileei *feedback* azkarra ematea da, kodean egindako aldaketek aurretik zeuden funtzionalitateak puskatu ez ditugula jakiteko. Hiru test mota azaltzen dira piramide hontan. Azpiko geruzan, Test Unitarioak daude, Integrazio testak tarteko geruzan, eta azkenik, goiko geruzan Muturretik-Muturrerako (*E2E* edo *End-to-end*) testak aurkitzen ditugu.

<figure class="align-center">
  <a href="#"><img src="{{ '/images/test-pyramid.jpg' | absolute_url }}" alt=""></a>
  <figcaption><a href="https://en.wikipedia.org/wiki/Mike_Cohn">Mike Cohn</a>-en Testen Piramidea.</figcaption>
</figure>

Goazen pixka bat sakonago ikustera zertan datzan test mota bakoitzak:

#### TEST UNITARIOAK   

Test Unitarioek gure kode-basearen funtzionalitate edo osagai txiki bat frogatzeaz arduratzen dira. Helburua, kode unitate horrek, modu isolatuan, behar den bezela jokatzen duela egiaztatzea da. Orokorrean kode unitate hau funtzio edo metodo bat izango da.

Test mota hauek oso azkarrak eta *merkeak* dira, eta osagai oso txikiak frogatzen dituztenez, gure test-suiteko zati haundiena berauez osaturik egongo da. Beraien garrantzia dela eta (baita TDD praktikatzeko ordurako), aurrerago sakonago aztertuko ditugu test mota hauen ezaugarriak.

Dena den, Test Unitarioekin ez da nahikoa aplikazio baten funtzionamendu zuzena aztertzeko, besteak beste, ez dutelako azpiegiturako geruzarekiko harremana frogatzen. Hori dela eta, beharrezkoa da hurrengo test mota.

#### INTEGRAZIO TESTAK   

Integrazio testek kode unitate bat baino gehiagok elkarren artean behar bezela funtzionatzen dutela frogatzen dute, hauek multzoka frogatuz. Test hauetan ohikoa da azpiegiturako geruza ere tartean sartzea, izan datu basera dei bat, HTTP eskaera bat edo dena delakoa.

Datu baseen kasuan, garrantzitsua da test hauek produkzioan edukiko genukeen bezelako datu base baten aurka frogatzea. Noski, honek datu base lokal bat edukitzea suposatzen du, eta beste bat gure *build pipelinean* (aplikazioa eraikitzeko prozesuan). Hontan, Docker bezelako tresnek lagun gaitzakete, datu base ezberdinen *irudiak* modu errez batean edukitzea ahalbidetzen bait digu.

Kanpo-zerbitzuekiko integrazioa ere frogatu beharko litzateke. Test mota hauei Zerbitzu testak ere deitzen zaie. Hauek gure kodeak kanpo-zerbitzuekin, esaterako REST API batekin, nola elkarreragiten duen frogatzen dute.

Pentsatuko duzuen bezela, test hauek motelagoak dira, eta gainera, kasu gutxiago gain hartzen dituzte, hori dela eta, piramidearen erdian kokatzen dira.

#### MUTURRETIK-MUTURRERAKO (E2E) TESTAK 

*E2E (End to End)* edo muturretik-muturrerako testak, software guztia hasieratik amaierara balioztatzen dute, kanpoko interfazeekin integratzearekin batera. Muturretik muturrerako (E2E) testen helburua software osoa probatzea da, bai menpekotasunak frogatzeko, bai datuen integritatea bermatzeko, bai beste sistema, interfaze eta datu-baseekiko komunikazio egiaztatzeko, produkziokoa bezelakoa den eszenario bat birsortuz. Onarpen test ere esaten zaie funtzionalitate bat bere osotasunean, hau da ziklo osoa, frogatzen dutelako, produkzioko egoera batean gertatuko litzatekeen moduan.

Test hauek noski, denetan motelenak dira, eta prestatzen gehien kostatzen dutenak, hori dela eta, gutxienekoak izango dira, eta piramidearen goian aurkitzen ditugu.

#### TEST AUTOMATIKOEN EGITURA

Test automatikoak idazterakoan, egitura zehatz bat jarraituko dugu. Egitura hau 3 pausotan banatzen da:

- **TESTUINGURUA PRESTATU**   
  Test motaren arabera gauze gehiago egin beharko dira pausu honetan. Izan daiteke klase bat instanziatzea, Datu Base bateko datuak modu jakin batean prestatzea, edo TDDko postan ikusiko dugun moduan, zerbitzuren baterako deiak simulatzea (mockeatzea).

- **FROGATU NAHI DEN UNITATEA EXEKUTATU**   
  Azalpen gutxi behar du honek. Exekutatu nahi dugun unitatea (orokorrean funtzio edo metodo bat), dagozkion parametroekin exekutatu eta itzultzen duen balorea (itzultzen badu) jasotzean datza.

- **EMAITZA EGIAZTATU**   
  Exekuzioaren emaitza (itzulerako balioa edo sortu duen albo-ondorioa) egiaztatzean datza, esperotako portaera eman dela frogatuz.

Hiru pauso hauei *Arrange*, *Act*, *Assert* (AAA) edo *Given*, *Then*, *When* deitzen zaie normalean.

Ondoko kode adibidea ***Java*** lengoaian eta *xUnit* familiako ***JUnit*** test liburutegia erabiliz dago idatzia:

```java
public class CalculatorShould {

  @Test
  void sum_two_numbers() {
    // Arrange or Given
    Calculator calculator = new Calculator();
    // Act or When
    Integer result = calculator.sum(3, 5);
    // Assert or Then
    assertThat(result).isEqualTo(8);
  }
}
```

Beste adibide hau berriz ***JavaScript*** erabiliz eta *xSpec* familiako ***Mocha*** test liburutegia erabiliz dago idatzia:

```javascript
describe('Calculator should' () => {
  it('sum two numbers' () => {
    // Arrange or Given
    const calculator = new Calculator();
    // Act or When
    const result = calculator.sum(3, 5);
    // Assert or Then
    expect(result).to.equal(8);
  });
});
```

Ikusten duzuen moduan, xUnit motako liburutegien kasuan, Test Unitatea klase bat da, eta hau izendatzeko klasearen izena erabiltzen dugu. Klasearen baitan, *@Test* atributoa daukan metodo bakoitza berriz Test bat litzateke, eta testa deskribatzeko, metodoaren izena erabili beharko genuke. Irakurterrezagoa eta semantikoagoa izateko, testetako metodoen izenak *snake case* erabiliz idazteko joera dago, luzeak izan ohi bait dira, nahiz eta gainontzeko metodo eta funtzioak Javan, konbentzioz, camel case erabiliz idazten diren.

xSpec familiko liburutegien kasuan berriz, Test Unitatea funtzio bat da, nun lehenengo parametroa honen izena edukiko duen textu bat den, eta bigarren parametroa ordea funtzio bat. Funtzio honen barruan, testak joango dira, eta hauek ere, funtzioak izango dira, nun lehen parametroa testa deskribatzen duen textu bat izango litzatekeen, ta bigarrena berriz, testaren gorputza osatuko duen funtzio bat. Famili hontako test liburutegien kasuan, semantikoa izatea lortzea errezagoa da, bai Test unitatearen izena eta bai test bakoitzaren deskribapena textu libreek osatzen bait dute.

#### TEST UNITARIOEN EZAUGARRIAK

Testak idaztea gure garapen prozesuaren parte baldin bada (dela TDD praktikatuz edo praktikatu gabe), zalantzarik gabe Test Unitarioak izango dira gehien idatziko ditugunak. Horregatik, garrantzitsua da hauek eduki behar dituzten zenbait ezaugarri kontuan izatea. Ezaugarri hauek gogoan izateko, zenbait akronimo existitzen dira. Horietako bat ***FIRST*** da:

- **Fast (azkarra):**   
  Testak azkarrak izan behar dira, ahalik eta azkarrenak, behar den maiztasunarekin exekutatuak izan daitezen. Testak motelak badira ez ditu behar adina aldiz exekutatuko eta orduan ez dute bere funtzioa behar bezala beteko.

- **Independent (independientea):**   
  Testak independenteak izan behar dute, hau da, gai zan beharko ginateke testak edozein ordenetan exekutatzeko, eta test batek fallatzeak ez lioke beste test bati eragin beharrik.

- **Repeteable (errepikagarria):**   
  Probak errepikagarriak izan behar dira edozein ingurunetan (dev, test, prod), emaitza aldakorrik gabe. Sare edo datu-base baten mende ez badaude, hauek, huts egiten duten probetarako arrazoi posible gisa deskartatuko dira, hauek probatzen ari garen klase edo metodoaren kodearen baitan soilik bait daude.

- **Self-validated (auto-balioztagarria):**   
  Probek autobalioztatzaileak izan behar dute, eta horrek esan nahi du pasatu edo fallatu egin behar dutela eta ez dutela inolako esku-hartzerik behar garatzaile, tester eta abarren aldetik. (adibidez, log bat begiratu behar izatea).

- **Timely (une egokian):**   
  Proba bat idazteko une egokiena zein den egiten dio erreferentzia azken honek, eta une hau, TDD aztertzerakoan ikusiko dugun moduan, justu produkzioko kodea idatzi aurretik da.

Nik hauei, beste bi gehituko nizkieke:

- **Mantentzen erreza:**   
  Gure testak, produkzioko kodea bailitzan tratatu behar ditugu, eta beraz, hauei aplikatuko genizkien praktika on guztiak aplikatu behar dizkiegu. Testak ahal den sinpleen edukitzen saiatu behar gara, errez ulertu eta mantendu ditzagun.

- **Ulerterreza:**   
  Test batek bere asmo edo helburua modu argian adierazi behar du, ta horretarako izen edo deskribapen aproposa aukeratzea ezinbestekoa da.

Honekin amaitutzat emango genuke Testen inguruko teoria. Hurrengo atal batean, **TDD** zer den eta nola erabiltzen den ikusiko dugu.