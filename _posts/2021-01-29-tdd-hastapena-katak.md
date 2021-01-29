---
layout: post
title: "TDD hastapena: Kode Katak"
categories:
  - TDD
tags:
  - TDD
  - Test
  - Programazioa
  - Kode Kata
---

TDD hastapenen inguruko post-multzoarekin jarraituz, gaurkoan Kode Katen inguruan arituko gara. Zer diren, zertarako erabiltzen diren, eta zenbait baliabide ikusiko ditugu.

Post-multzo honen aurreko atalak hemen aurki ditzakezu:
- [TDD hastapena: Testak](/tdd-hastapena-testak/)
- [TDD hastapena: TDD](/tdd-hastapena-tdd/)

{% include toc %}

# ZER DA KODE KATA BAT

Hasteko, saia gaiteze azaltzen zer den **Kode Kata** bat. Lehenik eta behin esan izenaren jatorria Japoniar arte martzialen Kata kontzeptuan dagoela. Arte martzialen munduan, Kata bat ezarritako mugimendu sekuentzia bat da eta bakarka edo bikoteka praktikatu ohi dira.

Programazio munduan, kode kata bat, praktika eta errepikapenaren bitartez programatzaileek beren gaitasunak hobetzera zuzendurik dagoen ariketa bat da. Ariketa hauek, kasu bakoitzean, helburu jakin batekin diseinatzen dira, dela TDD ikasteko, diseinu-patroi batekin praktikatzeko, lengoaia berri bat ikasteko, edo beste edozein praktika on edo printzipio aplikatzen ikasteko.

Mota ezberdinetako kode katak egongo dira ziurrenik, baina nik bi aipatuko ditut, nere ustez nagusienak hauek direlako. Bata Kata klasiko deituko dudana litzateke eta bestea Birfaktorizazio kata.

## KATA "KLASIKOAK"

Kata klasiko batean, hutsetik hasi beharreko ariketa bat proposatzen da. Esan bezela, kasu bakoitzean zailtasun maila ezberdina izan dezake, eta helburu jakin baterako pentsaturik egon. Kata hauek, orokorrean behintzat, TDD erabiliz ebazten dira, kasu batzuetan hau izanik helburu bakarra, hots, TDD praktikatzea.

Beste batzuetan, helburua SOLID, Diseinu Patroiak, algoritmo jakin bat, Objektuetara Bideratutako Programazioa, Polimorfismoa, etab... praktikatzea izan daiteke. Orokorren gehienez ordu para batean burutu daitezkeen ariketak izaten dira.

## BIRFAKTORIZAZIO KATAK

Birfaktorizazio kata baten normalean aurkitu ohi duguna honakoa da: jadanik garaturik dagoen programa bat, non ez dagoen test unitariorik, eta orokorrean gainera, ez den modu egokienean garaturik izan. Abiapuntua hau izanik, aldaketa bat egitea eskatzen zaigu, hau da, funtzionalitate berri bat gehitzea.

Lehen pausoa, funtzionalitatea gehitu aurretik, ezer apurtzen ez dugula ziurtatzeko, testak gehitzea izango litzateke. Hau da, ez genuke produkzioko koderik ikutuko, eta ahal ditugun portaera gehienak test bidez estaltzen saiatuko ginateke (ahal dela denak, baina ez da erreza izaten testak kodearen ondoren idazten direnean).

Behin hau egin dugula, ziurrenik funtzionalitate berria modu egokiago eta errezago batean gehitzeko, aurretik daukagun kodea birfaktorizatu beharko dugu, hau hedatzen errazagoa izan dadin.

Eta azken pauso bezela, funtzionalitate berriari dagozkion testak idazten joango ginateke, eta hauetako bakoitzan jarraian inplementatzen, test bakoitza berdean jarriz. Hau da, TDD erabiliz gehituko genuke funtzionalitate berria.

Kata hauek, *Legacy Code*-arekin lan egiten praktikatzeko izaten dira oso baliagarriak, 

# BALIABIDEAK

Edonork asma dezake Kata bat, azken finean, ariketa edo algoritmo bat ebaztea baino ez da, baina egun, jadanik "asmaturik" dauden eta kasu batzuetan ezagunak diren Katak existitzen dira. Gehienak egiazko gauzetan oinarriturik daude, eta beste batzuk abstraktoagoak dira, soilik ariketa modura asmatuak. Jarraian gutxi batzuk jarriko ditut adibide moduan, nolabait kategorizatuak.

## HASIBERRIENTZAKO KATAK

Kata bat baldin badago hasi berrientzat aproposa eta gainera oso ezagunea dena, hau [FizzBuzz kata](https://codingdojo.org/kata/FizzBuzz/) da. Honekin batera, [String Calculator kata](https://codingdojo.org/kata/StringCalculator/) ere oso egokia da, ez soilik garapen munduan hasten ari direnentzat, baizik eta garapen mundua esperientzia izan arren, TDDrekin hasi nahi dutenentzat. Hirugarren kata bat esan beharko banu hasteko ona izan daitekeena [Leap Years kata](https://codingdojo.org/kata/LeapYears/) litzateke.

Izan ere, hauek guztiek zerbait baldin badaukate komunean da eskakizunak oso definiturik dauzkatela, hau da, eskakizun bakoitza kasik test bat izango litzateke eta beraz, askotan hasieran edukitzen den zalantza haundienetako bat, hots, ze test idatzi, konponduta izango genuke.

## AURRERATUAGOENTZAKO KATAK

Garapen munduan adituagoak direnentzat eta TDDrekin esperientzai gehiago daukatenentzat, badira kata batzuk nahiko ezagunak direnak. Horien artean aurkitzen da esaterako [Tennis kata](https://codingdojo.org/kata/Tennis/) non helburua benetazko tenis partida bateko markagailua inplementatzea den. Oso ezaguna da ere [Bowling kata](https://codingdojo.org/kata/Bowling/) Uncle Bob-ek asmatu, edo ezagun egin zuena, eta bere *Agile Principles, Patterns, and Practices in C#* liburuan pausoz pauso kontatzen duena. Kata hauek aproposak dira objektuak modelatzen praktikatzeko.

Pare bat gehiago aipatzearren, [Mars Rover kata](https://kata-log.rocks/mars-rover-kata) ere nahiko ohikoa da eta baita [Conway's Game of Life](https://codingdojo.org/kata/GameOfLife/), urtero [Global Day Of Coderetreat](https://www.coderetreat.org/events/)-en egiten dena.

Ta azkenik, neri izugarri gustatzen zaidan kata bat *99 bottles of Beer* da. Izen bera duen [abestia](https://en.wikipedia.org/wiki/99_Bottles_of_Beer) array moduan itzultzean datza, baina benetan kata hontatik gehien gustatzen zaidana da nola dagoen pausoz pauso azaldua Sandi Metz-en [99 Bottles of OOP](https://sandimetz.com/99bottles) liburuan. 

## BIRFAKTORIZAZIO KATAK

Birfaktorizazio katen artean baten bat bada ezagun nik uste hori [Gilded Rose kata](https://codingdojo.org/kata/gilded-rose/) dela. Oso kata interesgarria da *Legacy* kodearekin lan egiten praktikatzeko eta soluzio pila aurkitu daitezke sarean lengoaia ezberdin askorekin ebatzia.

Bada oso interesgarria den beste birfaktorizazio kata bat eta hori [Trip Service kata](https://github.com/sandromancuso/trip-service-kata) da. Kata honen baliorik haundiena Sandro Mancusok berau ebazten daukan [bideo hau](https://www.youtube.com/watch?v=_NnElPO5BU0) da. Jada urte batzuk dituen arren, oso interesgarria da ikustea pausoz pauso nola garatzen duen eta erabaki bakoitzaren zergatia. Gomendagarria oso.

## KATAK BILATZEKO WEBAK

Bilaketa azkar bat eginez errez aurki daitezke katak dauzkaten webak baina nik hiru izendatuko ditut hemen, kata ezberdinak biltzen dituztelako eta kasu batzuetan gainera sailkaturik eta filtratzeko aukerarekin.

- [CodingDojo.org](https://codingdojo.org/kata/): Kata bilduma haundia dauka. Ez daude inola sailkaturik, baina soilik dauzkan Kata kopuruagaitik aipatzea merezi du.   

- [kata-log.rocks](https://kata-log.rocks/): Kata bilduma nahiko luzea dauka eta zailtasun zein beste zenbait gairengatik sailkatu daitezke Katak.  

- [katalyst.codurance.com](https://katalyst.codurance.com/browse): Codurance enpresak sorturiko Kata bilduma da. Ez da kata gehien dauzkana baina oso ongi sailkaturik daude, filtratu daitezke eta deskribapena oso landurik daude.

# KATAYUNOAK

Amaitzeko, Katayunoak aipatu nahi nituzke. Katayunoak Katak praktikatzeko antolatzen diren ekitaldi batzuk dira. Donostian sortu zuten programatzaile talde batek orain dela juxtu 10 urte eta azken urteetan [Gonzalo](https://twitter.com/gonzalo123) arduratzen da hauek antolatzeaz, euskal herria mailan behintzat.

Lanean oraindik Testak, TDD eta guzti hau praktikan jartzeko aukerarik eduki ez arren, hauengatiko interesa piztu zitzaidanean, Katayunoak oso garrantzitsuak izan ziren niretzat. Hauek aukera ematen zidaten kontzeptu "berri" hauekin esperimentatzeko, eta bide batez, nire interes berak zituen jendea ezagutzeko.

Ez naiz asko luzatuko zer diren zehazten, izan ere [weban](https://katayunos.com/) azalpen txiki bat daukazue eta bestela, Gonzalok berak esan ohi duen bezela, batetara joan eta goiza bukatzerakoan, bertan gertatutakoa izango da Katayuno bat.

Dena den, ideia bat egiteagatik, programatzaile talde bat toki batean (orokorrean enpresaren batek utzitako ofizina batean) elkartzen da, lehenengo gauza gosaldu eta ondoren kode Kata bat egiteko (hortik izena, gaztelerazko Kata + desayuno hitzen baturatik). TDD erabiltzen da eta bikoteka egiten da lan, ordenagailu bakarra erabiliz, hau da, [*Pair programming*](https://en.wikipedia.org/wiki/Pair_programming) eginez. Orokorrean moderatzaile edo bideratzaile bat egoten da eta berak esaten du zein kata egingo den eta ze baldintza berezi (baldin badaude) bete behar diren.

Hasiera batean donostian (edo inguruetan) egiten baziren ere, azken urteetan ohikoa da Donostia, Bilbo eta Iruña uztartzea. Orokorrean, urtean hiru edo lau antolatu ohi dira, eta aurtengoa, 10. denboraldia beharko luke. Egungo osasun egoera dela eta, 2020 otsailaren 29an izan zen azkenekoa, nik neuk bideratu nuena kasualitatea, eta ez dago argi noiz egin ahalko den berriz.

# LABURBILDUZ

Kode Katak ariketa oso interesgarriak dira programatzaile bezela gure egunerokoan egin ezin ditzakegun gauza edo froga asko egiteko, izan lengoaia berri bat ikasteko, TDD, SOLID, Clean Code eta horrelako printzipioak praktikan jartzeko, edo beste edozein helburutarako.

Nire asmoa, hurrengo post-ean kode Kata bat egin eta bideoz grabatzea da, mekanika nolakoa den modu zehatzago batean erakusteko.
