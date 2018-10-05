---
layout:      post
comments:    true
title:       "El que hi ha darrere del debat urbanístic de Sant Feliu"
author:      martibosch
date:        2017-04-30
category:    fait-a-sant-feliu
media:       Fet a Sant Feliu
media-link:  http://www.fetasantfeliu.cat/opinio/101185/el-que-hi-ha-darrere-del-debat-urbanistic-de-sant-feliu
media-date:  3 de maig del 2017
tags:        urbanisme
---

La meva família va arribar a Sant Feliu al 1995, quan [segons l'Idescat hi vivien 36736 persones](http://www.idescat.cat/). Recordo quan anava a l'escola Salvador Espriu i jugant a futbol al pati no es veia cap edifici més que les fàbriques del polígon Matacàs. Quan l'any 2011 vaig tornar-hi per a fer de monitor de l'activitat extraescolar de futbol sala, em va produir una sensació amarga veure el pati envoltat d'edificis de set o vuit plantes que li fan ombra durant l'hivern. De manera similar, gent de la meva edat de l'escola Bon Salvador m'havia explicat algun cop com abans de la construcció de la ciutat esportiva Joan Gamper, des de la seva escola a Sant Joan Despí no hi havia més que descampats.

Aquests records no haurien d'estranyar massa a ningú, ja que a data del 2016 Sant Feliu comptava amb 44086 habitants, el qual representa un creixement del 20% respecte el 1995. Hom pot entendre doncs, que l'ampliació de la superfície urbana de Sant Feliu és una mera resposta a un creixement demogràfic, que de fet ha estat inferior al 26% corresponent a la mitjana catalana. El debat més calent que hi ha sobre la taula, segons les opinions que he llegit, és la manera en la que aquests nous edificis contribueixen a fer Sant Feliu una ciutat dormitori.

El que intento explicar a continuació, és com la transformació que porta a Sant Feliu cap a una ciutat dormitori no és més que el resultat d'un procés demogràfic que es manifesta arreu del món en diverses magnituds i variants, però compartint gran part dels mateixos fonaments.

## Com es distribueix la població catalana?

L'arrel del problema s'amaga sota les distribucions estadístiques de la població a Catalunya. Per a començar amb un exemple, mireu el gràfic següent:

![Distribució d'alçades i pesos](/assets/images/urban_complexity/height_weight_distplot.png "Distribució d'alçades i pesos"){: .center-image }

A l'esquerra i a la dreta es mostren respectivament les distribucions d'alçada i pes d'un grup d'individus, segons [una enquesta de joves de 18 anys a Hong Kong](http://wiki.stat.ucla.edu/socr/index.php/SOCR_Data_Dinov_020108_HeightsWeights). Veiem doncs que la gran majoria d'individus de l'estudi mesuren entre 165 i 180 centímetres i pesen entre 45 i 70 kilograms, amb mitjanes entre els 170-175cm i 55-60kg respectivament. El que s'observa correspon una *distribució normal*, que és raonablement com estem acostumats a pensar quan parlem de números. Per exemple, quan comparàvem notes a la classe de l'institut, probablement la mitjana estava entre el 5 i el 6, mentre que la majoria tenia notes entre el 3 i el 7. Desprès quedaven els pocs estudiants que havien tret entre un 1 i un 2 i a l'altre extrem també pocs estudiants que tenien un 9 o un 10.

Si ara observem com es distribueixen els habitants en els diferents municipis de Catalunya segons l'Idescat, ens trobem amb el següent:

![Distribució de poblacions municipals de Catalunya](/assets/images/urban_complexity/municipal_population_distplot.png "Distribució de poblacions municipals de Catalunya")

La diferència entre aquesta distribució i les d'alçada i pes mostrades abans són evidents. La gran majoria de municipis es situen a l'esquerra del gràfic, amb poblacions molt petites, mentre que hi ha grup pràcticament inapreciable de municipis de més de 200000 habitants. El que s'observa correspon a una distribució *lognormal* [amb una cua aproximable per una *power-law* segons la *Llei de Zipf*](https://github.com/martibosch/martibosch.github.io/blob/master/assets/notebooks/urbanism_sant_feliu.ipynb). El lingüista estatunidenc George Kingsley Zipf va popularitzar aquesta llei a l'observar que en la gran majoria de paraules d'un llenguatge s'utilitzen rarament, ja que hi ha un grup reduït de paraules que utilitzem molt sovint. Hi ha doncs una clara analogia amb el nostre gràfics: la gran majoria de municipis són poc poblats, mentre que hi ha un grup petit de municipis amb molts habitants. De fet, aquest comportament es replica en molts altres fenòmens de la ciència, com ara sistemes biològics, termodinàmics o conjunts fractals en matemàtiques. Una pràctica habitual per analitzar millor aquests fenòmens és representar-los en escala logarítmica. Si ho apliquem al nostre gràfic, trobem el següent:

![Distribució logarítmica de poblacions municipals de Catalunya](/assets/images/urban_complexity/municipal_population_log_distplot.png "Distribució logarítmica de poblacions municipals de Catalunya")

S'observa així que la majoria de les poblacions tenen entre 100 i 10000 habitants, amb un únic municipi de més d'un milió d'habitants situat a l'extrem dret, que com bé podem imaginar correspon a la ciutat de Barcelona. Malgrat ser tant recurrents, el que sobta d'aquestes distribucions és la dificultat que ens comporta intentar predir o extrapolar-ne conclusions de manera intuïtiva. Per exemple, sabent que mesuro 177cm, tots seríem més o menys capaços d'estimar intuïtivament i amb relativa precisió que un 25% de les persones de la meva edat deuen ser més alts que jo. En canvi, malgrat que ens diguin que a Barcelona hi ha el 21% dels habitants de Catalunya, segurament no sabrem estimar tant fàcilment quin percentatge dels Catalans viuen en les 5 ciutats més grans (són un 33%, entre Barcelona, l'Hospitalet de Llobregat, Badalona, Terrassa i Sabadell).


## Què ens ha portat a aquesta desproporció?

Qualsevol persona que conegui més o menys Catalunya es pot imaginar que el creixement demogràfic no s'ha repartit de manera simètrica. Probablement, els habitants de tercera edat la vida dels quals ha transcorregut en els petits pobles de la Catalunya interior ens podrien explicar el progressiu èxode dels seus fills i néts cap a poblacions més grans de la comarca o directament cap a Barcelona. Aquesta migració succeïx de manera retroactiva: com que les poblacions més grans ofereixen més oportunitats, més habitants nous hi arriben, que a la vegada fan créixer el municipi, que passa a oferir encara més oportunitats.

Aquest comportament es coneix com a avantatge acumulatiu o *preferential attachment*, i es pot resumir en l'aforisme de "els més rics es tornen més rics, i els més pobres més pobres". La formalització d'aquest fenomen s'atribueix a les observacions del matemàtic escocès Udny Yule sobre el fet que els gèneres biològics amb més espècies són també els que tenen més capacitat de generar noves espècies.  

Veiem un exemple de com funciona el *preferential attachment*, il·lustrat amb una xarxa d'amics, on cada node representa una persona, i un enllaç entre dos nodes simbolitza que les persones corresponents són amics. La situació inicial, il·lustrada en la imatge a continuació, és una persona central que és l'única amistat dels altres 9 membres. 

![Estat inicial de la xarxa](/assets/images/urban_complexity/preferential_attachment_01.png "Estat inicial de la xarxa")

Per a simular el *preferential attachment*, farem que arribin noves persones a la xarxa, que intentaran fer-se amics d'algú que ja tingui molts amics. Els detalls exactes de la implementació estan descrits [en la documentació del programa utilitzat](http://ccl.northwestern.edu/netlogo/models/PreferentialAttachment). Veiem doncs com evoluciona el sistema quan hi ha 25, 50, 100, 250 i 500 persones respectivament:

![Evolució de la xarxa segons el "preferential attachment"](/assets/images/urban_complexity/preferential_attachment.gif 'Evolució de la xarxa segons el "preferential attachment"')

Podem observar doncs, que una idea molt senzilla com el *preferential attachment* pot generar patrons clarament estructurats. Les imatges de la xarxa probablement ens recordin a diferents fenòmens: enllaços entre pàgines web, amistats de Facebook, connexions entre Aeroports, xarxes de proteïnes en biologia o matèria condensada en física. Aquest tipus de xarxes agrupen sota el nom de *scale-free networks*, i comparteixen propietats molt similars en camps completament diferents de la ciència. Les xarxes que formen les ciutats i pobles, no en són una excepció.

Com es pot observar, a mesura que evoluciona el sistema, la persona central acapara gran part de les amistats. De fet, la distribució del numero d'amics que té cada persona de la xarxa, (representada en el gràfic de "Degree Distribution") es va assemblant cada cop més a la distribució en municipis dels habitants de Catalunya. Al cap i a la fi, les decisions que guien l'evolució de les ciutats segueixen el mateix raonament que els nouvinguts a la xarxa: quan decidim mudar-nos a una ciutat, la majoria busquem aquelles que ens ofereixin més oportunitats, de la mateixa manera que els nouvinguts busquen fer-se amics d'aquells que tenen més amics.

## L'Àrea Metropolitana de Barcelona i l'*urban sprawl*

Segons el raonament del *preferential attachment*, la ciutat de Barcelona hauria d'haver acaparat la majoria del creixement de Catalunya durant els últims anys. Però si mirem com ha evolucionat la població de les comarques de Catalunya en termes absoluts des del 1981 al 2011 segons l'Idescat, podrem observar com no ha estat així. 

<iframe width="100%" height="640" frameborder="0" src="https://martibosch.carto.com/builder/f215ab9d-c7f8-4577-9d2f-7f371519f254/embed" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe>

Veiem doncs que el Barcelonès és justament la comarca que més població ha perdut, concretament 208211 habitants. En canvi, les comarques de l'Àrea Metropolitana de Barcelona (AMB) i el Tarragonès són les que més han crescut. La hipòtesi més versemblant seria que ja no queda espai per allotjar nous residents al Barcelonès, i per tant aquests s'estableixen a les comarques de l'AMB. Però no només no s'ha pogut allotjar nous residents, sinó que hi ha hagut una pèrdua de població significativa. Això implica que força residents del Barcelonès que han decidit marxar-hi, molt possiblement per a establir-se en poblacions veïnes de l'AMB. 

Aquesta migració cap a les rodalies normalment s'atribueix a les preferències per un context residencial més tranquil, espaiós i verd, evitant la congestió de les ciutats sense allunyar-se'n excessivament. La seva prevalença en el context nord americà ha popularitzat globalment el terme *urban sprawl*, que es refereix al model urbà de zones residencials disperses, separades dels centres d'activitats d'oci i comercials, sovint forçant als seus habitants a una forta dependència del transport motoritzat. Probablement això ens faci pensar a localitats del Vallès, el Maresme o del Baix Llobregat, com ara Sant Cugat, Cerdanyola, el Masnou, Mongat, Vallirana, Corbera, i també a barris Santfeliuencs recents com ara Les Grasses, Mas Lluhí o els nous habitatges al costat de la Ciutat Esportiva Joan Gamper.

## Implicacions a l'escala Sant Feliu i de Catalunya

El que descriu aquest article doncs, es pot recapitular en les següents observacions:

* la distribució demogràfica de Catalunya està molt descompensada, amb una minoria de municipis concentrant la major part de la seva població
* aquesta distribució és resultat d'on procés retroalimentat que tendeix a desproporcionar-la encara més (el *preferential attachment*), amb Barcelona com a gran centre de gravetat
* la saturació de la ciutat de Barcelona, i les preferències per un context residencial més tranquil, han desencadenat una migració dels seus habitants cap a altres municipis de l'AMB

Com hom pot imaginar, aquestes observacions són extrapolables a la majoria de regions desenvolupades del món: segons [les previsions United Nations](https://esa.un.org/unpd/wup/), la proporció mundial de persones vivint en ciutats ha passat d'un 3% al 1800 a un 50% actual, amb estimació d'arribar al 60% al 2030. Seguint l'actual tendència global cap a l'*urban sprawl*, [s'espera que la superfície total ocupada per aglomeracions es tripliqui els propers 40 anys](https://www.theguardian.com/cities/2016/jul/12/urban-sprawl-how-cities-grow-change-sustainability-urban-age).

Considerant la magnitud d'aquest procés demogràfic, cal entendre que probablement hi ha poques solucions implementables des de l'ajuntament de Sant Feliu. Els municipis metropolitans estem condemnats a jugar un paper clau en aquesta transformació, i per a no acabar sent satèl·lits invisibles de Barcelona, cal canviar la percepció territorial de Catalunya. Com remarca el periodista Marc Andreu en [un extret del seu llibre *Les ciutats invisibles*](http://www.elcritic.cat/blogs/sentitcritic/2016/05/19/les-ciutats-invisibles-de-la-catalunya-metropolitana/), "Que en el concepte periodístic i polític de territori, tant interioritzat a Catalunya, no s’hi incloguin les realitats metropolitanes només s’explica com una vacuna mal receptada contra el centralisme de Barcelona". D'altra banda, potser també són necessàries altres respostes a tal centralisme, com ara solucions a l'èxode juvenil dels pobles de la Catalunya oblidada ["d'on als 18 anys marxes a estudiar i rarament hi tornes"](http://www.elcritic.cat/blogs/sentitcritic/2016/02/05/guia-per-a-catalans-que-baixareu-el-7-f-a-la-colonia-de-mes-enlla-de-lebre/), ja que en molts casos tals joves no s'estableixen a l'AMB per voluntat pròpia, sinó per manca d'oportunitats en la seva comarca d'origen.

Però inclús acceptant que la migració cap a l'AMB és inevitable, cal plantejar quin tipus de teixit urbà acomodarà aquest creixement. Un [estudi de l'Universitat Autònoma de Barcelona](http://www.sciencedirect.com/science/article/pii/S0169204607002848) va monitoritzar les transformacions de l'AMB entre el 1993 i el 2000, destacant com la proliferació de zones industrials i comercials segregades junt amb la privatització d'espais públics ens estan acostant cada cop més al model urbà nord americà. Ara bé, l'estudi distingeix una característica peculiar de l'AMB: l'existència de centres vitals a les afores de Barcelona ha frenat notablement l'*urban sprawl*, ja que sembla que els seus habitants tenim més preferència per habitar a prop d'aquests centres, a diferència dels països anglosaxons. El que potser podem fer des de Sant Feliu doncs, és aprofitar aquesta diferència per a evitar acabar sent una ciutat dormitori: [com deia l'Arnau Picón al Fet a Sant Feliu](http://www.fetasantfeliu.cat/opinio/100126/sant-feliu-ciutat-invisible-a-la-recerca-de-la-cohesio-social), "Més enllà de les polítiques municipals necessàries, comença a ser hora que la ciutat s’espavili. Els qui més hi hem de fer som les entitats, els comerços i els ciutadans".

No entraré a valorar quines són les decisions de l'ajuntament en clau urbanística que ens poden portar al model de ciutat dormitori o d'*urban sprawl*. Ara bé, el que sí que hem de reclamar tots els Santfeliuencs és el nostre dret a participar activament en aquest debat i la presa de decisions, ja que votar un cop cada quatre anys no és suficient per a hipotecar el model de ciutat que volem per als propers anys.
