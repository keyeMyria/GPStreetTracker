**********************************************************************************************
*Java Framework zur Bachelorarbeit: Automatisierung der Kartographierung von Stra�enschildern*
*Autor:                             Philipp Unger                                            *
*Datum:                             11.03.2013                                               *
*Hochschule:                        Universit�t Passau                                       *
**********************************************************************************************

GPStreetTracker ist ein Java Framework zur Objekterkennung in Videos.

Es bietet ein Plugin zur farb- und formbasierten Erkennung von Verkehrschildern.

Zur Bildverarbeitung werden dabei auch Methoden aus den OpenCV Bibliotheken verwendet.

Zur Verwendung des Programms muss eine funktionierende OpenCV_2.4.3,
sowie eine JavaCV_0.3 Installation vorliegen.

    Genauere Informationen zur Installation von OpenCV auf Ihrem Betriebssystem befinden
    sich auf der Entwicklerhomepage:

    http://www.opencv.org/

    oder unter:

    http://opencv.willowgarage.com/wiki/InstallGuide

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Funktionsweise des Frameworks:

Das Framework beteht aus 3 Bereichen.

	- Der Videoverarbeitung
	- Den Plugins
	- und der GPS-Verarbeitung

1. Videoverarbeitung:

	Hier werden die einzelnen Frames aus dem Video ausgelesen und nacheinander
	an die Plugins �begeben.

2. Plugins

	In den Plugins, welche alle das gleiche Interface implementieren, findet
	die Objekterkennung statt. Hier k�nnen beliebige Algorithmen auf den
	Videoframe zur Objekterkennung angewendet werden.

3. GPS-Verarbeitung

	Hier wird der GPX-Track eingelesen und die erkannten Objekte der Ortskoordinate
	zugewiesen, in der sie aufgetreten sind.
	Danach k�nnen die Bilder der erkannten Objekte, sowie ein dazugeh�riger Waypoint
	im GPX-Track, gespeichert werden.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

F�r das Framework wurde bereits ein Plugin zur Stra�enschilderkennung, namens
'HoughSignRecognition', entwickelt.

Funktionsweise des Plugins 'HoughSignRecognition':

Die Stra�enschilder werden anhand ihrer Farbe und Form erkannt.
Die geschieht im Wesentlichen in 2 Schritten:

1. Bildaufbereitung und Filterung

	In der Bildaufbereitung werden als erstes die Farbintensit�tswerte der
	zu untersuchenden Bilder erh�ht, um die Farben des Bildes besser auszupr�gen.
	Somit heben sich die monochromen Verkehrsschilder noch besser von ihrer Umgebung ab.
	Zudem wird so durch die teilweise Eliminierung von zu hellen und zu dunklen
	Stellen ein St�rfaktor ausgeglichen, welcher durch ungleichm��ige Lichtverh�ltnisse
	entsteht. Dies geschieht �ber Pixelwert-manipulation im HSV Farbraum.
	Danach werden die aufbereiteten Bilder, mittels eines Schwellwertverfahrens
	in Bin�rbilder umgewandelt. Das Bin�rbild stellt somit eine Filtermaske des
	im Schwellwertverfahren angewandten Farbwertes dar.

2. Zusammenf�hrung und Segmentierung

	Nach der Filterung werden die einzelnen Farbmasken (Bin�rbilder) zu einem Bin�rbild
	zusammengef�hrt, welches nun eine Maske aller gefilterten Farben darstellt.
	Anschlie�end wird das Bin�rbild, anhand von zusammenh�ngenden Fl�chen,
	segmentiert und diese Segmente der Formerkennung �bergeben.

3. Formerkennung

	Als letzter Schritt werden die gefundenen Segmente mit Hilfe von Hough-Transformationen
	auf die geometrischen Formen von Stra�enschildern untersucht. Um die Erkennungsrate
	hierbei zu steigern, wird auf den Bin�rbildsegmenten vor der Hough Transformation
	ein Gau�scher Weichzeichner angewendet, um eventuelle L�cken, die durch fehlerhafte
	Farbinformationen der Pixel der Originalbilder zustande gekommen sind, auszugleichen.



Die genaue Funktion und Vorgehensweise kann den Kommentaren im Quellcode entnommen werden.