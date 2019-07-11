# AbsInt

Implementiert einen "Seamless Wireless Channel Switch"

Bedient sich SDN-Verfahren. Benötigt Open vSwitch (OVS)

Argumente:

\# absint cut <device>
# absint establish <device>
# absint useports {Devices}
# absint netman adhoc [start | stop | set]
# absint netman adhoc set [freq | chan | essid]
# absint debug <einzelne Teilfunktionen>
# absint auto [controller | autonom]
# absint auto <Decision-Mode> -||<[Inter-AI-Connection-Mode][Connection-Address-Source]>||
# absint auto <Decision-Mode>-||Optionenk wlandevs {WLANPorts} -||Optionen||
• Inter-AI-Connection-Mode: [cfg | auto | server | client]
Default, wenn ausgelassen: [cfg]
→ Zur Kommunikation zwischen zwei AIs wird eine direkte Socket Verbindung aufgebaut.
Diese Option gibt an, ob das startende AI für diese Verbindung der akzeptierende
Server oder der verbindungsanfragende Klient sein soll, bzw. von welcher Stelle diese
Information bezogen werden soll.
cfg - Der Wert wird beim Start aus dem Conﬁg-File gelesen.
auto - Beteiligte AIs versuchen eigenständig eines zum Server zu bestimmen. Die Verbliebenen / das Verbleibende agiert als Klient.
server - Das AI erwartet andere AIs, die eine Verbindung aufzunehmen ersuchen.
client - Das AI versucht sich mit einem aktiven Server zu verbinden.
• Connection-Address-Source: [cfg | auto | cmdline]
Default, wenn ausgelassen: [cfg]
→ Diese Option kann nur gemeinsam mit und direkt anschließend an [Inter-AI-Connection-Mode] übergeben werden und ist in ihrer Bedeutung von dessen Wert abhängig.
Im Betrieb als Klient speziﬁziert sie die IPv4-Adresse, des Inter-AI-KommunikationServers, mit dem er sich verbinden soll. Im Betrieb als Server kann die Akzeptanz von eingehenden Verbindungen auf eine Adresse beschränkt werden. Dabei besteht die Möglichkeit der Übergabe der Wildcard Adresse 0.0.0.0.
cfg - Die Adresse des Verbindungspartners wird aus dem Conﬁg-File ausgelesen.
auto - Im Falle eines Servers wird die Wildcard Adresse 0.0.0.0 verwendet.
Im Falle eines Klienten wird das gesamte Subnetz im Netzwerk nach aktiven Servern durchsucht.
cmdline - Die zu verwendende Adresse wird direkt im Anschluss übergeben.
  
  
Funktionalität:

absint cut <device> & absint establish <device>
Können benutzt werden, um Ports zu deaktivieren oder wieder zu reaktivieren. Bei diesem Vorgang wird ein WLAN-Adapter zunächst deaktiviert, dann sein IfType geändert und anschließend der Adapter wieder reaktiviert.
  
absint useports {Devices}
Lässt bestimmen, welche Ports vom Abstract Interface verwaltet werden sollen. Kann durch Übergabe der absoluten Komplement Menge verwendet werden, um Ports von der Verwaltung durch das AI zu exkludieren.

absint netman adhoc [start | stop | set]
absint netman – der Netzwerk Manager (Net-Manager) des AI – ist der Teil von AbsInt, der direkt das Netzwerk steuert. Er konﬁguriert die Ad-hoc Netzwerke, über welche die AIs wireless miteinander verbunden werden, startet und beendet sie. Der netman kann wie aufgezeigt auch isoliert verwendet werden.

absint debug <einzelne Teilfunktionen>
Anschließend an das debug Kommandozeilen-Argument aufrufbar sind weitere intern verwendete Teilfunktionen zu ﬁnden. Diese dürften als Einzelaufruf marginal zuträglich jedoch bei Entwicklungsvorgängen wertvoll sein.
Beispielsweise umfasst diese Sammlung zwei Funktionen, die zum einen durch einen Ping Sweep über die Subnetze aller verbundenen Netzwerk-Adapter die ARP Tabellen des Systems komplettieren, respektive diejenigen Einträge auf der Konsole ausgeben, die nicht als FAILED gekennzeichnet sind.
  
absint auto <Decision-Mode> -||Optionen||
Startet den Dauerbetrieb, der die eigentliche Intention des AI darstellt.
  
wlandevs {WLANPorts}
Stellt ein optionales Argument dar, das es erlaubt der startenden AI-Instanz explizit die WLAN-Devices zu übergeben, die es für WLAN-Verbindungen mit Vermögen zum »Bruchlosen Kanal-Wechsel« aufwenden soll.
Die Übergabe kann an beliebiger Stelle nach „auto <Decision-Mode>“ erfolgen (Bsp.: auto controller wlandevs wlan0 wlan3). Entfällt das Argument, ermittelt AbsInt automatisiert alle vorhandenen WLAN-Interfaces und erwählt sich selbstständig welche zur Verwaltung der Funkverbindungen.
