` `**HWE Übungen 4CHEL** 	Rupp/Mayrhofer	BI\_2021-22 



||||
| :- | :-: | :-: |
|<p></p><p>**Aktiver Tastkopf**</p>|
||
|<p>![](docs/pic01.png)![](docs/pic02.png)</p><p></p><p></p><p></p>|
||
|<p>Projektmitglieder:</p><p>- Joel Rupp</p><p>- Sebastian Mayrhofer</p><p></p><p></p><p></p><p></p><p></p>|
|**HTL Rankweil**|**4CHEL**|**Schuljahr 2022-23**|

# **Aufgabenbeschreibung**

Im Projekt „Aktiver Tastkopf“ wurde versucht einen aktiven differentiellen FET-Tastkopf zu bauen. Die Anforderungen wurden bewusst hoch gesetzt, da uns der Schaltplan und das Layout für einen „single-ended-Tastkopf“ bereits geliefert wurden. Wir haben dann die Schaltung weiterentwickelt und quasi verdoppelt, um differentielle Messungen zu ermöglichen. 




# Inhaltsverzeichnis

[TOC]


# Liste Entwicklungstools

Microsoft Word: Version 2210 

Altium Designer: Version 22

LTspice: XVII 

Fusion360: V.2.014569 


# **Teil 1 - Allgemeines**

# Gewünschte Spezifikationen der Schaltung
Maximale Eingangsspannung der Schaltung: 1V Peak

Bandbreite: mindestens 1GHz

Messmethode: Differentiell

Eingangskapazität: <1pF


# Gewählte Bauteile
Als FET wurde der BF998 vorgegeben. Dieser wird in der DIY-Community oft verwendet um FET-Tastköpfe zu bauen. Er wird zwar nicht mehr produziert, jedoch sind noch riesige Altbestände verfügbar. 

Für den OP wurde der THS4302 gewählt. Dieser bietet eine groß genuge Bandbreite um die gewünschten Spezifikationen zu erfüllen. Der Preis ist zwar etwas hoch, aber so ist das nun mal bei HF-Bauteilen. 

Als Eingangskapazität zur AC-Kopplung wurden 1pF gewählt. Dies musste gemacht werden da die Schaltung mit größeren Werten in der Simulation schwang, und weil die Eingangskapazität sonst zu hoch wäre. 

# Erwartete Probleme
Zu erwartende Probleme sind:

- Schwingen der Schaltung
- Probleme mit den Lieferzeiten der Bauteile
- Zu starke Dämpfung der Schaltung

# Aufgetretene Probleme
Leiterplatte und Bauteile kamen viel zu spät an -> kaum Zeit zum testen

Leiterplatte konnte nicht planmäßig bestückt werden, da der dafür benötigte Raum belegt war

Schaltung schwingt manchmal

Nach dem FET lagen nur noch 5VDC an, wahrscheinlich wurde das Bauteil zerstört

Die Dämpfung nach der AC-Kopplung ist zu hoch

# Potentielle Verbesserungen der Schaltung

Anstatt einer Abschwächung von 20dB könnte relativ einfach eine Verstärkung von 20dB erreicht werden, was das SNR verbessern würde.


# **Teil 2 – Technische Details** 

# Was ist ein aktiver Tastkopf?
Der prinzipielle Unterschied zwischen einem passiven und einem aktiven Tastkopf liegt darin, dass der aktive Tastkopf eine Spannungsversorgung benötigt. Der Passive hingegen kann einfach an den Signaleingang des Messgeräts angeschlossen werden, und funktioniert dann auch schon.

Man kann die aktiven Tastköpfe grob in drei Kategorien einteilen:

- Tastköpfe mit Verstärkung

Diese Tastköpfe verstärken das gemessene Signal. Dies ist besonders bei sehr kleinen Signalen wichtig, da die meisten Messgeräte nur bis ca. 1mV/Div(z.B. Keysight InfiniiVision 3000G) die volle Bandbreite zulassen. Falls dann sehr schnelle, sehr kleine Signale gemossen werden müssen, wird man mit dem konventionellen passiven Tastkopf schnell an die Grenzen des Oszilloskopes stoßen. In diesem Fall kann man das Signal einfach 100-fach verstärken, und hat somit die volle Bandbreite zur Verfügung.

- Active FET Probes

Bei Active FET-Probes wird an den Eingang ein FET geschaltet, um dadurch extrem kleine Eingangskapazitäten zu erhalten. Zum Beispiel erreicht der N2750A-Tastkopf von Keysight 770fF (Femtofarad) am Eingang. Dies ist aus mehreren Gründen praktisch. Einerseits wird somit das DUT kapazitiv nicht so stark belastet. Besonders bei Quarzen ist dies wichtig, denn diese werden mit wenigen Pikofarad geladen, wenn dann durch den Tastkopf die Kapazität steigt, hört er auf zu Schwingen, und die Messung ist sinnlos. Andererseits hat dies den Vorteil, dass dadurch die Eingangsimpedanz im hohen Frequenzband besser ausfällt. Denn Tastköpfe mit hoher Eingangskapazität bieten bei hohen Frequenzen kaum mehr Widerstand. Durch die FET-Eingangsstufe bleibt der Tastkopf auch im hohen Frequenzspektrum relativ hochohmig und belastet das DUT dadurch nicht stark.

- Differentialtastkopf

Normalerweise wird beim Messen mit dem Oszilloskop immer gegen die Masse des Oszilloskops gemessen. Wenn aber nun zwischen zwei Punkten im Dut gemessen werden soll, hat man ein Problem. Denn nun wird immer einer der beiden Punkte auf die Masse kurzgeschlossen, was sowohl beim DUT als auch beim Messgerät zur Zerstörung führen kann, und somit keine Messung ermöglicht. Hier muss mit einem Differentiellen Tastkopf gemossen werden. Dieser misst die Different zwischen beidem Messpunkten, und liefert so das richtige Signal


# ![](docs/pic03.png)

*Blockschaltbild der Schaltung*



# **Teil 3 – Erklärung der Schaltung**

 **Die FET-Eingangsstufe mit Abschwächung**

![](docs/pic04.png)

Das zu messende Signal kommt von links, und geht über C5 (1pF) in das zweite Gate des FETs hinein. Oben wird über R1 (6k8) und R2 (4k7) die Versorgungsspannung auf ca. 2V geteilt, und über R3 (10M) auf das selbe Gate geleitet. Dadurch wird das Gate etwas „vorgespannt“. Aus der Source kommt dann das Messsignal, mit einem Offset vom ca ~0,8V bis ~0.9V. Dann wird das Signal mit R6 (2R4) und R9 (24R) noch durch 10 geteilt. Über C6 (100nF) und C7 (1nF) wird dann der Offset entfernt, und dieser Teil der Schaltung DC-mäßig abgekoppelt. J1, J5 und J4 sind Testpunkte zum späteren Messen der Schaltung. 

#

**Die Verstärkung**

 ![](docs/pic05.png)



Von links kommt das Signal, welches aus der Eingangstufe kommt, also zehnfach abgeschwächt. Von oben wird mit R4 (10k) und R10 (10k) ein Offset vom 2.5V auf den invertierten Eingang des OPs (THS4302) geleitet. Dieser ist als invertierter Verstärker aufgebaut. Damit kann über R8 (10k Potentiometer) und R11 (1k) exakt auf 10x Abschwächung abgeglichen werden. Mit C8 (10µF) wird sichergestellt dass nur der AC-Teil des Signals verstärkt bzw. abgeschwächt wird. 


#

**Die Differenzierung**

 ![](docs/pic06.png)


Über R14 (1k) kommt das Signal der Schaltung für die negative Messspitze herein, und über R18 (1k) das für den positiven Teil. Die Schaltung ist als Differenzverstärker aufgebaut, mit keiner Verstärkung für beide Eingänge, da alle Widerstände den gleichen Wert haben. 	

#

**Der USB-C Power delivery Standard**

Die Spannungsversorgung der Schaltung erfolgt über USB-C. Um stabile 5V mit maximal einem Ampere zu erhalten, muss jeweils ein Widerstand an CC1 und CC2 angeschlossen werden, CC steht dabei für Control Channel. Damit wird dem anderen angeschlossenem Gerät mitgeteilt, welcher beider Geräte die Source ist, und wer die Sink ist. Außerdem wird gesteuert welche Spannung und wie viel Strom maximal zur Verfügung gestellt wird. Bei uns wurden 5k6 bestückt, was bedeutet: 

- Das angeschlossene Device ist ein Sink
- Es werden 5V benötigt
- Es werden maximal 1A benötigt

![](docs/pic07.png)![](docs/pic08.png)


# **Teil 4 – Simulation**


Um beim testen der Schaltung keine bösen Überraschungen erleben zu müssen, wurde die Schaltung mit LTspice simuliert. Im Internet wurde ein Simulationsmodel für den BF998 FET gefunden, welcher großen Anklang in der DIY-Community findet. Nun die Ergebnisse für ein 80mVPP 10MHz Eingangssignal auf beiden Kanälen, jedoch mit 180° verschoben




# Zeitbereich

## Eingangssignal:
![](docs/pic09.png)


Man sieht dass die Signale 180° Phasendifferenz besitzen. 


## Signal nach C1:

![](docs/pic10.png)



Das Signal wird mit einem 2.043V Offset belegt, und etwas abgeschwächt. Die Abschwächung ist zwar ungewollt, kann aber später mit dem Potentiometer ausgeglichen werden. 

## Signal nach dem FET:

![](docs/pic11.png)



Das Signal wird weiter abgeschwächt. Etwas Offset bleibt bestehen, ca. 970mV.

## Signal am Eingang des OPs:

![](docs/pic12.png)


Das Signal wurde nun noch etwas weiter abgeschwächt auf ca 10mVPP, und mit einem 2.5V Offset belegt. 

## Signal nach dem OP:

![](docs/pic13.png)



Das Ausgangssignal wurde nun verstärkt. Die Schaltung ist in der Simulation noch nicht richtig eingestellt, was man an der Ausgangsspannung 16mVPP bemerkt. 


## Ausgangssignal:

![](docs/pic14.png)



Das Ausgangssignal der gesamten Schaltung. Man sieht wieder, wie die Schaltung noch nicht ganz optimal eingestellt ist.



## Bandbreite

# ![](docs/pic15.png)



Man kann erkennen dass die Schaltung von 100kHz bis ca 1GHz innerhalb von ±3dB bleibt

# **Teil 5 - Messung**

# Stromaufnahme
![](docs/pic16.jpeg)Da uns ein OP fehlte, konnte die Schaltung nur bis zur Differenzierung gemossen werden. Zuerst wurde das Netzgerät auf 5V mit 200mA Strombegrenzung eingestellt, da die geschätzte Stromaufnahme bei etwas über 100mA lag. Die gemossene Stromaufnahme waren 114mA.

# Zeitbereich
![](docs/pic17.png)

*Dann wurde am negativen Eingang ein 10MHz Signal mit 200mVPP eingespeist.*

Das gelbe Signal ist das Eingangssignal, welches direkt vom Signalgenerator ins Oszilloskop geleitet wurde. Das blaue Signal wurde mit einem 10x Tastkopf gemossen, welcher zwar im Oszilloskop eingestellt wurde, bei der internen Messung des Oszilloskops aber aus unerfindlichen Gründen nicht berücksichtigt wurde. Gemossen wurde direkt vor dem FET. Hier lagen nur noch 20.4mV an. Dies ist eine etwas stärkere Dämpfung als erwartet. 

![](docs/pic18.png)

*Nach dem FET lagen 5V DC an:* 

Dies sieht danach aus als ob der FET kaputt gegangen ist, und die Spannung vom ersten Gate welches mit 5V versorgt wird, direkt an den Ausgang weiterleitet. 

**Aufgrund des Zeitmangels kann diesem Fehler leider nicht weiter nachgegangen werden!**

# Messung der Bandbreite


Mithilfe eines Netzwerkanalyzers könnte die Bandbreite, als auch die Kennlinie bestimmt werden. Dies wäre wahrscheinlich mithilfe der Streuparameter gemacht worden, spezielle mit S21. Da die Schaltung aber nicht komplett ist, und die Zeit schlicht und einfach fehlt, kann dies nicht durchgeführt werden. 


# **Teil 6 – Technische Unterlagen**

#  Schaltplan

![](docs/pic19.png)


# Board Top
![](docs/pic20.png)

# Board Bottom

![](docs/pic21.png)

# BOM

![](docs/pic22.png)





# Gehäuse
![](docs/pic23.png)![](docs/pic24.png)


