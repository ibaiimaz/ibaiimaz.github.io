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

# ZER DIRA TESTAK?

Software munduan testing-a, garatu dugun (edo garatzen ari garen) sistemak bete behar dituen eskakizunak betetzen dituela egiaztatzeko teknika multzoari esaten zaio.

Test hauek mota ezberdinetakoak izan daitezke. Lehen sailkapen bat, hauek exekutatzeko moduaren arabera egin dezakegu, hau da, eskuz edo modu automatikoan exekutatzen diren aztertuz.

## ESKUZKO TESTAK

Eskuzko testing-a kasu batzuk prestatu eta beharrezko elementuak eskuz exekutatzean datza. Test hauek noski muga asko dauzkate. Eskuz egiten direnez motelak dira, gainera kasu ezberdinak prestatu behar ditugu baina hauek behin baino gehiagotan exekutatu ahal izateko erreplikatzen zailak dira, beraz, aldi berean garestiak direla esan daiteke, ta ez dira oso fidagarriak, kasuak ahaztu bait ditzakegu, eta batez ere, kasu guztiak betetzea oso zaila delako.

Noski, kasu batzuetan beharrazkoak eta lagungarriak dira, baina zuzenean, kasu zehatz horietaz QA taldea arduratzea da. 

## TEST AUTOMATIKOAK

Test bat software zati txiki bat da, normalean funtzio bat, beste software unitate bat exekutatzen duena eta espero dugun emaitza edo efektua sortzen den egiaztatzen duena. Hau guztia egin ahal izateko liburutegi ezberdinak existitzen dira lengoaia eta teknologiaren arabera.

Test automatikoen artean ere mota ezberdinak bereiz ditzakegu.

### TEST EZ-FUNTZIONALAK

Test ez-funtzionalen helburua da sistema batek nola funtzionatzen duen epaitzeko erabil daitezkeen irizpideak zehazten dituen baldintza bat egiaztatzea, hala nola erabilgarritasuna, irisgarritasuna, mantengarritasuna, segurtasuna eta/edo errendimendua. Hau da, sistemak nola erantzuten duen egiaztatzen dute.

Mota ezberdinetako testak aurkitu ditzakegu multzo honetan baina ez dugu hauetan sakonduko. Dena den, aipatzearren, honakoak aurki genitzazke: Abiatura-testak, Karga-testak, Erabilgarritasun-testak, Segurtasun-testak, etab...

### TEST FUNTZIONALAK

Test funtzionalek gure aplikazioaren zati batek *negozioko* jendeak esandako eskakizunak betetzen dituela, eta gainea errorerik ez duela egiaztatzen dute. Test funtzionalen artean mota ezberdinetakoak daude, baina gu ondorengoetan zentratuko gara.

#### TEST UNITARIOAK

Test Unitarioek kode unitate baten funtzionamendu zuzena modu isolatuan frogatzen dute. Orokorrean kode unitate hau funtzio edo metodo bat izango da.

#### INTEGRAZIO TESTAK

Integrazio testek kode unitate bat baino gehiagok elkarren artean behar bezela funtzionatzen dutela frogatzen dute, hauek multzoka frogatuz. Test hauetan ohikoa da infraestrukturako geruza ere tartean sartzea, izan datu basera dei bat, HTTP eskaera bat edo dena delakoa.

#### E2E EDO ONARPEN TESTAK

*E2E (End to End)* edo onarpen testek, software guztia hasieratik amaierara balioztatzen dute, kanpoko interfazeekin integratzearekin batera. Muturretik muturrerako (E2E) testen helburua software osoa probatzea da, bai menpekotasunentzako, bai datuen integritaterako, bai beste sistema, interfaze eta datu-baseekiko komunikaziorako, produkziokoa bezelako eszenario bat birsortuz.

Onarpen test ere esaten zaie funtzionalitate bat bere osotasunean, hau da ziklo osoa, frogatzen dutelako, produkzioko egoera batean gertatuko litzatekeen moduan.

### TESTEN PIRAMIDEA


<figure class="align-center">
  <a href="#"><img src="{{ '/images/test-pyramid.jpg' | absolute_url }}" alt=""></a>
  <figcaption>Look at 580 x 300 <a href="#">getting some</a> love.</figcaption>
</figure>