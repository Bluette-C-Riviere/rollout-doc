---
slug: /posdealers/rollout-doc/middleware/step-by-step/2-hardware-tses-am-lokalen-server-fuer-mehrere-kassen
title: Leitfaden f�r "Hardware-TSE(s) am lokalen Server f�r mehrere Kassen"
---

# Hardware TSE am lokalen Server f�r mehrere Kassen

Dieser Leitfaden f�hrt Sie durch ein geteiltes Szenario, wo eine SCU/TSE �ber mehrere Queues und Kassen geteilt wird.



![](../../images/server-width-hw-tse.png)



## 0 - Voraussetzungen
Als H�ndler stellen Sie bitte sicher, dass Sie zuerst die **Surrogate Funktion** benutzt haben, um in das **Konto Ihres Kunden zu wechseln und �nderungen in diesem Kontext auszuf�hren**.

Dazu klicken Sie bitte linkerhand `PosOperator` - `�bersicht`, suchen das Konto Ihres Kunden und klicken auf den dazugeh�rigen Namen.

*Sollte es nicht m�glich sein den Namen anzuklicken, �berpr�fen Sie bitte das Statusicon und stellen sicher, dass Ihr Kunde bereits aktiv geschalten ist (ansonsten h�tte er m�glicherweise sein Kontoerstellung noch nicht abgeschlossen).*

![](../images/surrogate.png)



Sobald Sie in das Konto Ihres Kunden eingestiegen sind, erhalten Sie rechts oben einen roten Knopf mit der Aufschrift `Zu Ihrem Account wechseln`. Dies zeigt einerseits an, dass Sie erfolgreich in das Konto Ihres Kunden gewechselt haben, andererseits k�nnen Sie damit zu Ihrem eigenen Konto zur�ckwechseln.

![](../images/switch-back.png)





## 1 - Erstellung der SCU
### Schritt 1 - Navigation im Men�
W�hlen Sie linkerhand im Men� `Konfiguration` - `TSE / Signatur-Erstellungs-Einheit`.

![menu_scu](../images/menu-scu.png)

### Schritt 2 - Hinzuf�gen

Klicken Sie den `Hinzuf�gen` Knopf.

![scu_add](../images/scu-add.png)

### Schritt 3 - Eingabe der Details

Geben Sie einen aussagekr�ftigen Namen f�r Ihre neue SCU im Feld Beschreibung an, w�hlen Sie das korrekte Paket und die neueste Version f�r Ihre konkrete TSE und w�hlen Sie das Outlet, in welchem diese SCU aktiv sein soll. Nach Auswahl vom Paket wird das Portal automatisch die aktuellste Paketversion als Standard ausw�hlen.

![main](../images/main-scu.png)



Klicken Sie `Speichern` sobald Sie alle Informationen eingegeben habe. Danach werden Sie zu einem Schirm umgeleitet, wo Sie zus�tzliche technische Details konfigurieren k�nnen. Die Mehrheit dieser Einstellungen ist TSE-spezifisch (*im folgenden Beispiel haben wir eine Fiskaly Cloud TSE* ausgew�hlt) und erfordert die korrekten Werte f�r diese TSE (z.B. den richtigen Laufwerksbuchstaben oder den korrekten API Schl�ssel).
*Eine vollst�ndige Liste aller m�glichen Optionen f�r jeden TSE Typ kann [hier](https://docs.fiskaltrust.cloud/de/docs/posdealers/rollout-doc/middleware#templating-zum-anlegen-von-cashboxen) im Abschnitt `SCU` gefunden werden.*

Ein weiterer wichtiger Punkt hier ist die URL, unter welcher die SCU f�r die Queue verf�gbar sein soll. Diese besteht aus drei Elementen

1. dem **Kommunikationsprotokoll**. Sie k�nnen zwischen gRPC, REST, SOAP und net.pipe w�hlen
2. dem **Hostname**. Stellen Sie sicher hier eine Adresse zu verwenden, welche von all Ihren Queues erreichbar ist
3. dem f�r die SCU spezifischen **URL Pfad**

Klicken Sie auf einen der verf�gbaren Protokolltypen um automatische eine passende URL daf�r zu generieren.



> **Es ist wichtig, dass Ihre SCU URL nicht den gleichen Netzwerk-Port verwendet wie die URL, welche Sie sp�ter f�r Ihre Queue verwenden m�chten.**



![](../images/scu-config.png)

Sobald Sie die Konfiguration abgeschlossen haben scrollen Sie zum Seitenende, klicken `Speichern und schliessen` und die neue SCU sollte auf dem folgenden Schirm unter den SCUs Ihres Kunden aufgef�hrt sein.



## 2 - Erstellung der Cashbox f�r die SCU

### Schritt 1 - Navigation im Men�

W�hlen Sie linkerhand im Men� `Konfiguration` - `CashBox`.

![](../images/menu-cashbox.png)



### Schritt 2 - Hinzuf�gen

Klicken Sie den `Hinzuf�gen` Knopf.

![](../images/cashbox-add.png)



### Schritt 3 - Eingabe der Details

Geben Sie einen aussagekr�ftigen Namen f�r Ihre neue SCU Cashbox im Feld Beschreibung an und w�hlen Sie das Outlet, in welchem diese Cashbox aktiv sein soll aus.

![](../images/main-cashbox.png)

Klicken Sie auf `Speichern` sobald Sie alle Informationen eingegeben haben und die neue Cashbox sollte auf dem folgenden Schirm unter den Cashboxen Ihres Kunden aufgef�hrt sein.



### Schritt 4 - Hinzuf�gen der SCU

Zur�ck bei der Liste der Cashbox, klicken Sie den `Bearbeiten per Liste` Knopf.

![](../images/edit-cashbox.png)



Auf dem folgenden Schirm m�ssen Sie die vorher erstellte SCU f�r diese Cashbox ausw�hlen, indem Sie die richtige Zeile anw�hlen.

![](../images/select-scu.png)



Klicken Sie auf `Speichern`.



## 2 - Erstellung der Queue

### Schritt 1 - Navigation im Men�

W�hlen Sie linkerhand im Men� `Konfiguration` - `Queue`.

![menu_queue](../images/menu-queue.png)

### Schritt 2 - Hinzuf�gen

Klicken Sie den `Hinzuf�gen` Knopf.

![](../images/queue-add.png)

### Schritt 3 - Eingabe der Details

Geben Sie einen aussagekr�ftigen Namen f�r Ihre neue Queue im Feld Beschreibung an, w�hlen Sie das korrekte Paket f�r den gew�nschten Datenbanktyp, sowie die neueste Version (*wenn Sie SQLite w�hlen, wird die Middleware die Datenbank automatisch erstellen*), w�hlen Sie den gew�nschten Timeout Wert aus (Standard sind 15.000 Millisekunden), geben Sie die Serien- oder Identifikationsnummer Ihrer Kassa an (*oder w�hlen Sie `Generieren der Identifikation` aus um das Portal eine eindeutige Nummer generieren zu lassen*) und w�hlen Sie das Outlet, in welchem diese Queue aktiv ist aus.

![main_queue](../images/main-queue.png)

Sobald Sie alle Informationen eingetragen haben, scrollen Sie ans untere Ende der Seite und klicken `Speichern`, worauf Sie auf einen Schirm umgeleitet werden, wo Sie zus�tzliche technische Details konfigurieren k�nnen. Der wichtigste Teil hier, ist die URL unter welcher die Queue IPOS Anfragen von Ihrer Kassa entgegennehmen soll. Diese URL besteht aus drei Elementen

1. dem **Kommunikationsprotokoll**. Sie k�nnen zwischen gRPC, REST, SOAP und net.pipe w�hlen
2. dem **Hostname** (w�hlen Sie `localhost` falls die Queue auf derselben Maschine l�uft wie Ihr Kassasystem)
3. dem f�r die Queue spezifischen **URL Pfad**

Klicken Sie auf einen der verf�gbaren Protokolltypen um automatische eine passende URL daf�r zu generieren. Sie k�nnen die Standardwerte beibelassen, ausser Sie ben�tigen hier spezifische Werte.



> **Es ist wichtig, dass Ihre Queue URL nicht den gleichen Netzwerk-Port verwendet wie die URL welche Sie im vorigen Schritt f�r die SCU verwendet haben.**



![](../images/queue-config.png)

Klicken Sie `Speichern und schliessen` sobald Sie die Konfiguration abgeschlossen haben. Die neue Queue sollte dann auf dem folgenden Schirm unter den Queues Ihres Kunden aufgef�hrt sein.





## 4 - Erstellung der Cashbox f�r die Queue

### Schritt 1 - Navigation im Men�

W�hlen Sie linkerhand im Men� `Konfiguration` - `CashBox`.

![](../images/menu-cashbox.png)



### Schritt 2 - Hinzuf�gen

Klicken Sie den `Hinzuf�gen` Knopf.

![](../images/cashbox-add.png)



### Schritt 3 - Eingabe der Details

Geben Sie einen aussagekr�ftigen Namen f�r Ihre neue Cashbox im Feld Beschreibung an und w�hlen Sie das Outlet, in welchem diese Cashbox aktiv sein soll aus.

![](../images/main-cashbox.png)

Klicken Sie auf `Speichern` sobald Sie alle Informationen eingegeben haben und die neue Cashbox sollte auf dem folgenden Schirm unter den Cashboxen Ihres Kunden aufgef�hrt sein.



### Schritt 4 - Hinzuf�gen der Queue

Zur�ck bei der Liste der Cashbox, klicken Sie den `Bearbeiten per Liste` Knopf.

![](../images/edit-cashbox.png)



 Auf dem folgenden Schirm m�ssen Sie die vorher erstellte Queue f�r diese Cashbox ausw�hlen, indem Sie die richtige Zeile anw�hlen.

![](../images/select-queue.png)



Zus�tzlich m�ssen Sie die **Queue mit der SCU verbinden**, indem Sie das Pop-Out Icon auf der rechten Seite anklicken. Dies �ffnet folgenden Schirm, wo Sie die vorher erstellte SCU ausw�hlen und `Speichern und schliessen` anklicken.

![](../images/connect-scu.png)



## 5 - Erstellung von Queues f�r zus�tzliche Kassen

Wiederholen Sie 3 und 4 f�r jede zus�tzliche Kassa, auf welcher Sie eine Queue installieren, und mit der SCU verbinden, m�chten.



## 4 - Cashbox Build

Dies ist ein **wichtiger Schritt**, welchen Sie immer durchf�hren m�ssen nachdem Sie eine Cashbox erstellt, **oder ge�ndert**, haben.

Um ein Build, oder Rebuild, einer Cashbox durchzuf�hren, �ffnen Sie die Liste aller Ihrer Cashboxen und klicken den `Rebuild configuration` Knopf bei der entsprechenden Cashbox, sofern dieser in Orange markiert ist.

![](../images/build-cashbox.png)

Der Build Prozess ist relativ schnell und innerhalb weniger Sekunden fertig. Sobald der Rebuild Knopf grau ist haben Sie eine aktuelle Konfiguration.



## Zusammenfassung

An diesem Punkt sollten Sie folgende Komponenten haben

* **eine Cashbox mit einer SCU**, verbunden mit Ihrer TSE
* **eine Anzahl von Cashboxen, jeweils mit einer Queue** (und einer integrierten Datenbank, falls Sie SQLite gew�hlt haben), welche die SCU ben�tzen

Sie k�nnen nun den Launcher f�r Ihre SCU herunterladen und auf der Maschine ausrollen, welche f�r die SCU vorgesehen ist (z.B. wo die Hardware TSE angeschlossen ist), sowie alle anderen Launcher die f�r Ihre unterschiedlichen Kassen bestimmt sind.
