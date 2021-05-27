---
slug: /posdealers/rollout-doc/middleware/step-by-step/6-data-center-as-operational-environment
title: Leitfaden f�r "Rechenzentrum als Operational Environment"
---

# Rechenzentrum als Operational Environment

Dieser Leitfaden f�hrt Sie durch ein Setup wo Ihre Outlets Kassenterminals verwenden und durch ein zentrales Kassensystem in Ihrem Rechenzentrum verrechnen. Weiters f�hrt das Kassensystem f�r jedes Outlet eine eigene Cashbox (mit eigener Queue und SCU), welche jeweils abh�ngig davon von welchem Outlet die Anfrage gekommen ist, angesprochen wird.



![](../../images/terminals-mw-cloud.png)



Diese Konfiguration beinhaltet eine Anzahl von traditionellen Queue/SCU Setups, jedoch mit dem gr�ssten Unterschied, dass diese *nicht die klassische integrierte SQLite Datenbank* verwenden, sondern **stattdessen eine externe MySQL Datenbank**.

In diesem Tutorial stellen wir zuerst sicher, dass die Datenbank funktionsf�hig ist. Danach setzen wir folgende Komponenten auf

1. eine SCU
2. eine Queue
3. eine Cashbox, welche die SCU und die Cashbox beinhaltet



## 0 - Voraussetzungen
Als H�ndler stellen Sie bitte sicher, dass Sie zuerst die **Surrogate Funktion** benutzt haben, um in das **Konto Ihres Kunden zu wechseln und �nderungen in diesem Kontext auszuf�hren**.

Dazu klicken Sie bitte linkerhand `PosOperator` - `�bersicht`, suchen das Konto Ihres Kunden und klicken auf den dazugeh�rigen Namen.

*Sollte es nicht m�glich sein den Namen anzuklicken, �berpr�fen Sie bitte das Statusicon und stellen sicher, dass Ihr Kunde bereits aktiv geschalten ist (ansonsten h�tte er m�glicherweise sein Kontoerstellung noch nicht abgeschlossen).*

![](../images/surrogate.png)



Sobald Sie in das Konto Ihres Kunden eingestiegen sind, erhalten Sie rechts oben einen roten Knopf mit der Aufschrift `Zu Ihrem Account wechseln`. Dies zeigt einerseits an, dass Sie erfolgreich in das Konto Ihres Kunden gewechselt haben, andererseits k�nnen Sie damit zu Ihrem eigenen Konto zur�ckwechseln.

![](../images/switch-back.png)



## 1 - �berpr�fung der Datenbankverbindung

Da wir keine integrierte SQLite Datenbank in diesem Tutorial verwenden, vergewissern Sie sich bitte, dass Ihr MySQL Instanz aktiv und korrekt f�r die jeweiligen Queue Setups konfiguriert ist. https://dev.mysql.com/doc/refman/8.0/en/ hat weitere konkrete Details zum MySQL Setup.



## 2 - Erstellung der SCU
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

1. dem **Kommunikationsprotokoll**. In einem Rechenzentrums Kontext ist es **wichtig hier `gRPC` auszuw�hlen**
2. dem **Hostname**. Vergewissern Sie sich, dass die hier ausgew�hlte Adresse von all Ihren Queues erreichbar ist
3. dem f�r die SCU spezifischen **URL Pfad**

Klicken Sie auf einen der verf�gbaren Protokolltypen um automatische eine passende URL daf�r zu generieren. Sie k�nnen die Standardwerte beibelassen, ausser Sie ben�tigen hier spezifische Werte.



> **Es ist wichtig, dass Ihre SCU URL nicht den gleichen Netzwerk-Port verwendet wie die URL, welche Sie sp�ter f�r Ihre Queue verwenden m�chten.**



![](../images/scu-config.png)

Sobald Sie die Konfiguration abgeschlossen haben scrollen Sie zum Seitenende, klicken `Speichern und schliessen` und die neue SCU sollte auf dem folgenden Schirm unter den SCUs Ihres Kunden aufgef�hrt sein.



## 3 - Erstellung der Queue

### Schritt 1 - Navigation im Men�

W�hlen Sie linkerhand im Men� `Konfiguration` - `Queue`.

![menu_queue](../images/menu-queue.png)

### Schritt 2 - Hinzuf�gen

Klicken Sie den `Hinzuf�gen` Knopf.

![](../images/queue-add.png)

### Step 3 - Entering the details

Geben Sie einen aussagekr�ftigen Namen f�r Ihre neue Queue im Feld Beschreibung an, w�hlen Sie `fiskaltrust.Middleware.Queue.MySQL` als gew�nschten Datenbanktyp, sowie die neueste Version, w�hlen Sie den gew�nschten Timeout Wert aus (Standard sind 15.000 Millisekunden), geben Sie die Serien- oder Identifikationsnummer Ihrer Kassa an (*oder w�hlen Sie `Generieren der Identifikation` aus um das Portal eine eindeutige Nummer generieren zu lassen*) und w�hlen Sie das Outlet, in welchem diese Queue aktiv ist aus.

![main_queue](../images/main-queue-mysql.png)

Sobald Sie alle Informationen eingetragen haben, scrollen Sie ans untere Ende der Seite und klicken `Speichern`, worauf Sie auf einen Schirm umgeleitet werden, wo Sie zus�tzliche technische Details konfigurieren k�nnen.

![](../images/queue-config-mysql.png)

Der wichtigste Teil hier der MySQL Connection String und die URL unter welcher die Queue IPOS Requests von Ihrer Kassa entgegenehmen soll.

#### MySQL Connection String

Bitte geben Sie hier einen g�ltigen .NET Connection String an, welcher alles Details f�r den Zugriff auf Ihre Datenbank beinhalt (z.B. Hostname, Benutzername, Password). Ein typischer Connection String w�re wie folgt formatiert

```
Server=myServerAddress;Database=myDataBase;Uid=myUsername;Pwd=myPassword;
```

Weitere Beispiele und Details finden Sie unter https://dev.mysql.com/doc/connector-net/en/connector-net-connections-string.html.

> Bitte stellen Sie auch sicher, dass jede Queue ihre eigene Datenbank hat, welche nicht mit anderen Queues geteilt wird.

#### Queue URL

Diese URL besteht aus drei Elementen

1. dem **Kommunikationsprotokoll**. In einem Rechenzentrums-Kontext **w�hlen Sie bitte hier `REST`**
2. dem **Hostname** (bitte w�hlen Sie hier den richtigen Hostname, unter welchem die Queue f�r das Kassensystem erreichbar sein wird)
3. dem f�r die Queue spezifischen **URL Pfad**

Klicken Sie auf einen der verf�gbaren Protokolltypen um automatische eine passende URL daf�r zu generieren.

Klicken Sie `Speichern und schliessen` sobald Sie die Konfiguration abgeschlossen haben. Die neue Queue sollte dann auf dem folgenden Schirm unter den Queues Ihres Kunden aufgef�hrt sein.



> **Es ist wichtig, dass Ihre Queue URL nicht den gleichen Netzwerk-Port verwendet wie die URL welche Sie im vorigen Schritt f�r die SCU verwendet haben.**



## 4 - Erstellung der Cashbox

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



### Schritt 4 - Verbindung der Komponenten

Zur�ck bei der Liste der Cashbox, klicken Sie den `Bearbeiten per Liste` Knopf.

![](../images/edit-cashbox.png)



Auf dem folgenden Schirm m�ssen Sie die vorher erstellte Queue f�r diese Cashbox ausw�hlen, indem Sie die richtige Zeile anw�hlen.

![](../images/select-queue.png)

Zus�tzlich m�ssen Sie die **Queue mit der SCU verbinden**, indem Sie das Pop-Out Icon auf der rechten Seite anklicken. Dies �ffnet folgenden Schirm, wo Sie die vorher erstellte SCU ausw�hlen und `Speichern und schliessen` anklicken.

![](../images/connect-scu.png)



Wir haben jetzt die Queue f�r die Cashbox konfiguriert und sie mit der SCU verbunden, m�ssen jedoch noch die SCU selbst der Cashbox hinzuf�gen, indem wir die richtige Zeile anw�hlen.

![](../images/select-scu.png)



Scrollen Sie an das untere Ende der Seite und klicken `Speichern`.



## 5 - Weitere Outlets

Wiederholen Sie die Punkte 2 bis 4 f�r jedes weitere Outlet, welches Sie konfigurieren m�chten und wo Sie Ihrem POS System eine Queue zur Verf�gung stellen m�chten.



## 6 - Cashbox Build

Dies ist ein **wichtiger Schritt**, welchen Sie immer durchf�hren m�ssen nachdem Sie eine Cashbox erstellt, **oder ge�ndert**, haben.

Um ein Build, oder Rebuild, einer Cashbox durchzuf�hren, �ffnen Sie die Liste aller Ihrer Cashboxen und klicken den `Rebuild configuration` Knopf bei der entsprechenden Cashbox, sofern dieser in Orange markiert ist.

![](../images/build-cashbox.png)

Der Build Prozess ist relativ schnell und innerhalb weniger Sekunden fertig. Sobald der Rebuild Knopf grau ist haben Sie eine aktuelle Konfiguration.



## Zusammenfassung

An diesem Punkt sollten Sie �ber eine Anzahl von Cashboxen f�r Ihr Setup verf�gen (jede mit ihrer eigenen Queue, SCU und TSE), wo jede Queue ihre **eigene MySQL Datenbank verwendet**. Nun ist es wichtig, dass Ihr POS System so konfiguriert ist, dass es jeweils die richtige Queue anspricht, wenn es einen Request von seinen Kassenterminals erh�lt.
