# Ma formation en informatique : bilan carbone personnalisé

$variable = variables récupérées pour un affichage personnalisé;
\_
\_
\_
\_

## SECTION 1 : Questions générales sur la formation

### Page 1 : Durée de la formation

1. Combien de temps dure votre formation (en jours) ?
   $durationTime

2. Combien d'heures par jour ?
   $hoursPerDay

3. Votre formation est-elle en total présentiel ?
   $isOnSite

### Page 2 : Remote time

if(!$isOnSite) ->

1. Combien de jours êtes-vous en distanciel ?
   $remoteTime

\_
\_
\_

## SECTION 2 : Déplacements

($isFullRemote = true)
if($remoteTime === $durationTime) -> skip

### Page 1 : Transport otpion

1. A quelle distance du site (en km) ?
   $distanceFromSite

2. Quel moyen de transport utilisez-vous majoritairement pour vous rendre sur site ?
   $transportOption = multi/covoit/solo

### Page 2 : Transport options details

switch ($transportOption)
case multi :
3. Quel transport utilisez-vous majoritairement pour vous rendre sur site ?
$transport = métro/bus/car/TER/TGV/Intercité

case covoit : 3. Quel type de voiture : thermique/electrique
$isCarElectric

4. Combien de covoitureurs?
   $covoitNb

case solo : 3. Quel transport utilisez-vous majoritairement pour vous rendre sur site ?
$transport = vélo/trottinnette élctrique/voiture elec/voiture thermique /moto

\_
\_
\_

## SECTION 3 : Locaux

if($isOnSite) -> skip

### Page 1 : Heating details

1. Avez-vous le DPE ?
   $hasDPE

2. A quelle température chauffez-vous votre logement ?
   $heatingTemperature

3. Quel est le cout de votre facture annuelle d'énergie ? (optionnel)
   $heatingAnnualCost

### Page 2 : Heating systems or DPE

if(!$hasDPE){
3. Quel est votre moyen principal de chauffage ?
$heatingSystem
}else {

3. Quel est le DPE ?
   $DPE = ABCDEFG

}

if($isOnSite) -> skip

\_
\_
\_

## SECTION 4 : Matériel informatique

1. Sur quel support informatique travaillez-vous ?
   $equipment: fixe/portable/tablet

\_
\_
\_
\_
\_
\_
\_
\_
\_

# Calculs à faire

## SECTION 2 : Déplacements

$distanceFromSite (en km)
$transportCarbon :
Le métro : 0,004 kg CO2e / km
Le bus : 1,13 kg CO2E / km
Le car : 0,29kg CO2E/km
vélos / trotinnettes à assistance éléctrique : 0,11 kg CO2E / km
TER : 0,028 kg CO2e / km
TGV : 0,03 kg CO2E/km
Intercité : 0,09kg CO2E/km

Les voitures / 2 roues:
Voiture éléctrique : 1.03kg CO2E/k
+1 covoitureureuse : 0.52 CO2E/km
+2 covoitureureuse : 0,34 CO2E/km
+3 covoitureureuse : 0,26 CO2E/km
+4 covoitureureuse : 0,21 CO2E/km

Voiture normale : 2,18kg CO2E/km
+1 covoitureureuse : 1,09kg CO2E/km
+2 covoitureureuse : 0,73kg CO2E/km
+3 covoitureureuse : 0,54kg CO2E/km
+4 covoitureureuse : 0,44kg CO2E/km

scooter / petite moto : 0,76kg CO2E/km
moto : 1,91kg CO2E/km

$carbonPrint = $transportCarbon x $distanceFromSite x ($durationTime - $remoteTime) x 2

## SECTION 3 : Locaux

$surface (en m2)
$remoteTime (en jours -> convertir en an)

if($hasDPE){
$DPE
DPE → Co2 /m² /an
Classe A : - de 6kg → 3kgCO2/m2/an
Classe B : de 6 à 10kg → 8kgCO2/m2/an
Classe C : de 11 à 20kg → 15kgCO2/m2/an
Classe D : de 21 à 35kg → 28kgCO2/m2/an
Classe E : de 36 à 55kg → 45kgCO2/m2/an
Classe F : de 56 à 80 kg → 68kgCO2/m2/an
Classe G : + de 80kg → 80kgCO2/m2/an
}else{
$heatingSystem
Pompe à chaleur → 4kgCO2/m2/an
Poele à granulés → 5,6kgCO2/m2/an
Poele à bois → 9,2kgCO2/m2/an
Chauffage éléctrique → 11,9kgCO2/m2/an
Chauffage via réseau de chaleur → 18,7kgCO2/m2/an
Chauffage au gaz → 39kgCO2/m2/an
Chauffage au fioul → 57,2kgCO2/m2/an
}

$carbonPrint = $coefCarbon x $surface x $remoteTime

## SECTION 4 : Matériel informatique

$equipmentEnergy
-fixe: 150 W -> 0.15 kW
-portable: 50 W -> 0.05kW
-tablet: 10 W -> 0.01kW

$equipmentEnergyTotal = $equipmentEnergy x $durationTime x $hoursPerDay

$coefCarbon = 0.5kg CO2/kWh (moyenne)

$carbonPrint = $coefCarbon x $equipmentEnergyTotal (kgCO2)

# Conclusion

Total des $carbonPrint
