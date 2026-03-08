[Gruppenfuehrer Fragen index.html](https://github.com/user-attachments/files/25828519/Gruppenfuehrer.Fragen.index.html)
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feuerwehr Quiz - 1:1 Original</title>
    <style>
        body { font-family: sans-serif; line-height: 1.4; padding: 15px; background: #f0f2f5; }
        .container { max-width: 650px; margin: auto; background: white; padding: 20px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .progress { font-size: 0.9em; color: #666; margin-bottom: 10px; font-weight: bold; }
        h2 { color: #d32f2f; margin-top: 0; font-size: 1.2em; }
        .question-text { font-weight: bold; margin-bottom: 15px; display: block; font-size: 1.1em; }
        .option { display: block; background: #f8f9fa; margin-bottom: 8px; padding: 10px 10px 10px 40px; border-radius: 6px; cursor: pointer; position: relative; border: 1px solid #ddd; }
        .option input { position: absolute; left: 12px; top: 12px; width: 18px; height: 18px; }
        .selected { border-color: #d32f2f; background: #fff5f5; }
        button { background: #d32f2f; color: white; border: none; padding: 12px; border-radius: 6px; cursor: pointer; width: 100%; font-size: 16px; font-weight: bold; margin-top: 10px; }
        #feedback { margin-top: 15px; padding: 12px; border-radius: 6px; display: none; font-weight: bold; }
        .correct { background: #d4edda; color: #155724; border: 1px solid #c3e6cb; }
        .wrong { background: #f8d7da; color: #721c24; border: 1px solid #f5c6cb; }
        #next-btn, #override-btn { background: #28a745; display: none; }
        #override-btn { background: #6c757d; font-size: 14px; margin-top: 5px; }
        .stats { text-align: center; padding: 20px; }
        .percent-box { font-size: 2.5em; font-weight: bold; color: #d32f2f; margin: 10px 0; }
    </style>
</head>
<body>

<div class="container">
    <div id="progress" class="progress">Frage 1 von 60</div>
    <div id="quiz-box">
        <span class="question-text" id="question-display"></span>
        <div id="options-display"></div>
        <button id="check-btn" onclick="checkAnswer()">Antwort prüfen</button>
        <div id="feedback"></div>
        <button id="override-btn" onclick="overrideCorrect()">Trotzdem als RICHTIG werten</button>
        <button id="next-btn" onclick="nextQuestion()">Nächste Frage</button>
    </div>
</div>

<script>
    const allQuestions = [
        { id: 1, q: "1 Welche der folgenden Aussagen sind richtig?", o: {a: "Die Feuerwehr ist eine gemeinnützige, der Nächstenhilfe dienende Einrichtung", b: "Die Feuerwehr ist ein gemeinnütziger Verein", c: "Die Feuerwehr ist ein Verein, ohne eigene Rechtspersönlichkeit", d: "Die Feuerwehr ist eine Einrichtung der Gemeinde"}, a: ["a", "d"] },
        { id: 2, q: "2 Welche Grundrechte können nach § 2 FwG BW eingeschränkt werden?", o: {a: "Freiheit der Person", b: "Meinungsfreiheit / Pressefreiheit", c: "Gleichheit vor dem Gesetz", d: "Versammlungsfreiheit", e: "Unverletzlichkeit der Wohnung", f: "Recht auf Eigentum"}, a: ["a", "e", "f"] },
        { id: 3, q: "3 Welche Aufgabe hat der Feuerwehrausschuss und in welchen Zeitabständen wird er gewählt?", o: {a: "Jahresabschlussübung vorbereiten", b: "Beratung des Feuerwehrkommandanten", c: "Beschaffung von Fahrzeugen", d: "Feuerwehrangehörige entlassen", e: "Unterstützung des Feuerwehrkommandanten", f: "Erstellung der Feuerwehrsatzung", g: "Drei Jahre", h: "Fünf Jahre"}, a: ["b", "e", "h"] },
        { id: 4, q: "4 Welche Aufgaben obliegen dem Land nach dem FwG BW?", o: {a: "Förderung der Aus- und Fortbildung", b: "Errichtung und Unterhaltung der Landesfeuerwehrschule", c: "Lohnfortzahlung bei Arbeitsunfähigkeit", d: "Haftpflichtversicherung für Feuerwehrangehörige", e: "Unterstützung der Gemeinden bei der Beschaffung der Ausrüstung", f: "Förderung der Normung und Forschung", g: "Bestellung des Kreisbrandmeisters"}, a: ["a", "b", "e", "f"] },
        { id: 5, q: "5 Welches sind die Dienstpflichten der ehrenamtlichen Feuerwehrangehörigen?", o: {a: "Am Dienst und an der Ausbildung pünktlich teilzunehmen", b: "Den Weisungen der Vorgesetzten zu folgen", c: "Sich vorbildlich und kameradschaftlich zu verhalten", d: "Bei Alarm mit dem Privat-Pkw direkt zur Einsatzstelle zu fahren", e: "Sich bei Alarm unverzüglich am Alarmplatz einzufinden", f: "Die technische Einsatzleitung zu übernehmen", g: "Hydrantenplan erstellen"}, a: ["a", "b", "c", "e"] },
        { id: 6, q: "6 Welche der folgenden Aussagen ist richtig?", o: {a: "Die Inanspruchnahme von Sonderrechten mit dem Privat-Pkw ist nur durch Hupen zulässig", b: "Der Fahrer eines Privat-Pkw muss auch bei Alarmfahrt die Bestimmungen der StVO einhalten", c: "Die Teilnahme am Anwesenheitsnachweis gehört zum Dienst", d: "Ein jährlicher Dienstplan ist Pflicht"}, a: ["b", "c", "d"] },
        { id: 7, q: "7 Wer entscheidet darüber, ob eine Katastrophe vorliegt?", o: {a: "Der Einsatzleiter", b: "Die Ortspolizeibehörde", c: "Die Katastrophenschutzbehörde", d: "Das Regierungspräsidium"}, a: ["c"] },
        { id: 8, q: "8 Welches sind die Katastrophenschutzbehörden nach dem LKatSG?", o: {a: "Die Berufsfeuerwehr", b: "Die Landratsämter und die Bürgermeister der Stadtkreise", c: "Die Landesregierung", d: "Die Regierungspräsidien", e: "Das Innenministerium"}, a: ["b", "d", "e"] },
        { id: 9, q: "9 Was versteht man unter dem Begriff „untere Explosionsgrenze“?", o: {a: "Minimale Temperatur", b: "Eine Explosion unter Erdgleiche", c: "Den minimalen Sauerstoffgehalt", d: "Die niedrigste Konzentration des brennbaren Stoffes in einem Gemisch"}, a: ["d"] },
        { id: 10, q: "10 Welche Stoffe gehören zur Brandklasse A?", o: {a: "Acetylen", b: "Aceton", c: "Ammoniak", d: "Autoreifen", e: "Baumwolle", f: "Holz (Hobelspäne)", g: "Bienenwachs"}, a: ["d", "e", "f"] },
        { id: 11, q: "11 Bei welchem der aufgeführten Löschmittel ist der Stickeffekt die Hauptlöschwirkung?", o: {a: "Wasser im Sprühstrahl", b: "Kohlenstoffdioxid", c: "Löschpulver B/C", d: "Löschpulver A/B/C/D", e: "Luftschaum mit Verschäumungszahl 75"}, a: ["b", "e"] },
        { id: 12, q: "12 Bei welchen Brandeinsätzen ist Wasser als Löschmittel nicht anzuwenden?", o: {a: "Schornsteinbrände", b: "Düngemittel", c: "Metallbrände", d: "Heustockbrände", e: "Schwelbrände", f: "Mineralölbrände"}, a: ["a", "c", "f"] },
        { id: 13, q: "13 Wodurch ist eine Kraft gekennzeichnet?", o: {a: "Größe der Kraft", b: "Geschwindigkeit", c: "Richtung der Kraft", d: "Dauer", e: "Angriffspunkt der Kraft"}, a: ["a", "c", "e"] },
        { id: 14, q: "14 Wie groß ist die Höchstlast L bei einem Mehrzweckzug Z 16 beim Ziehen von Lasten?", o: {a: "1600 kg", b: "3200 kg", c: "4800 kg", d: "6400 kg"}, a: ["b"] },
        { id: 15, q: "15 Wie groß muss der Kraftstoffvorrat für den Fahrbereich und den Nebenabtrieb sein?", o: {a: "200 km / 2 h", b: "300 km / 3 h", c: "300 km / 4 h", d: "400 km / 4 h"}, a: ["c"] },
        { id: 16, q: "16 Welche Pumpen sind für das Löschgruppenfahrzeug LF 10 genormt?", o: {a: "FPN 10/750", b: "FPN 10/1000", c: "TUP 3-1,5", d: "TP 8/1"}, a: ["b"] },
        { id: 17, q: "17 Wie groß ist der Inhalt des Löschwasserbehälters bei einem LF 20 nach der Norm mindestens?", o: {a: "800 Liter", b: "1200 Liter", c: "1600 Liter", d: "2500 Liter"}, a: ["c"] },
        { id: 18, q: "18 Wie müssen Sie reagieren, wenn Sie Fässer mit einer brennbaren Flüssigkeit finden?", o: {a: "Heraustragen", b: "Gefahrensymbole beschreiben", c: "Zustand beschreiben", d: "Kühlen", e: "Ablöschen", f: "Nicht ohne Not daran vorbeigehen"}, a: ["b", "c", "f"] },
        { id: 19, q: "19 Wann ist mit gefährlichen CO-Konzentrationen zu rechnen?", o: {a: "Nach einer Explosion", b: "Brände in geschlossenen Räumen bei Luftmangel", c: "Schwelbrände von Stoffen der Brandklasse A", d: "Brandklasse C"}, a: ["b", "c"] },
        { id: 20, q: "20 Welche der genannten Atemgifte wirken reizend oder ätzend?", o: {a: "Kohlenstoffdioxid", b: "Blausäure", c: "Chlor und Säuredämpfe", d: "Nitrose Gase", e: "Stickstoff und Methan"}, a: ["c", "d"] },
        { id: 21, q: "21 In welchen Flaschen befinden sich brennbare Gase?", o: {a: "Blau mit zwei weißen Kreisen auf der Flaschenschulter", b: "Kastanienbraun", c: "Grün", d: "Rot", e: "Grau", f: "Grau mit roter Schulter"}, a: ["b", "d", "f"] },
        { id: 22, q: "22 Eine Acetylen-Flasche ist über eine längere Zeit der Brandwärme ausgesetzt. Was müssen sie beachten?", o: {a: "Druckgefäßzerknall ist möglich", b: "Flasche fortlaufend kühlen", c: "Kann sich selbst wieder erwärmen", d: "24h lagern und kontrollieren", e: "Direkt weiterbenutzen", f: "Zum Füllwerk bringen", g: "Kennzeichnen und dem Füllwerk übergeben"}, a: ["a", "b", "c", "d", "g"] },
        { id: 23, q: "23 Welche Wärmeübertragung ist ohne ein Medium möglich?", o: {a: "Wärmeströmung", b: "Wärmestrahlung", c: "Wärmeleitung"}, a: ["b"] },
        { id: 24, q: "24 Welche Treibmittel und welche Arten von Feuerlöschern gibt es?", o: {a: "Luft/Helium/CO2", b: "Propan/Butan", c: "Flüssigkeitsbrandlöscher", d: "Aufladelöscher", e: "Dauerdrucklöscher", f: "Gaslöscher", g: "Giftlöscher", h: "Schlaglöscher", i: "Pulverlöscher"}, a: ["a", "d", "e", "f", "i"] },
        { id: 25, q: "25 Welche Löschmittel können in tragbaren Feuerlöschern enthalten sein?", o: {a: "Wasser mit Zusätzen", b: "Graugussspäne", c: "D-Löschpulver", d: "Schaum", e: "BC-Pulver", f: "ABC-Pulver", g: "CO2", h: "Sand", i: "Schweröl", j: "Wasser"}, a: ["a", "c", "d", "e", "f", "g", "j"] },
        { id: 26, q: "26 Welche Vorteile hat ein Ringleitungssystem?", o: {a: "Weniger Ablagerungen im Rohrnetz", b: "Kürzere Abschaltstrecken bei Rohrbruch", c: "Geringe Kosten", d: "Wasserzuführung von zwei Seiten"}, a: ["a", "b", "d"] },
        { id: 27, q: "27 Wie lautet die Faustformel für den Abstand zwischen zwei Verstärkerpumpen?", o: {a: "Ausgangsdruck / Verlust", b: "Gesamtdruck / Verlust", c: "verfügbarer Druck / Verlust * 100"}, a: ["c"] },
        { id: 28, q: "28 Wie groß ist die maximale Entfernung zum Hydranten bei 5,4 bar und 800 l/min?", o: {a: "250 Meter", b: "300 Meter", c: "320 Meter"}, a: ["b"] },
        { id: 29, q: "29 Wie viel Wasser liefert ein Unterflurhydrant?", o: {a: "NW x 7-10", b: "NW x 12-15", c: "NW x 15-17"}, a: ["a"] },
        { id: 30, q: "30 Welche Aufgaben hat der Wachhabende vor Beginn der Sicherheitswache?", o: {a: "Rettungsweg kontrollieren", b: "Sicherheitsposten einweisen", c: "Ordnungsdienst sicherstellen", d: "Alarmierungsmöglichkeit prüfen"}, a: ["a", "b", "d"] },
        { id: 31, q: "31 Welcher Klasse müssen Dekorationsmaterialien in Versammlungsräumen entsprechen?", o: {a: "B1", b: "Normal entflammbar", c: "Schwer entflammbar", d: "F 30"}, a: ["a", "c"] },
        { id: 32, q: "32 Wann ist die Standsicherheit eines Gebäudes gewährleistet?", o: {a: "Brandverhütungsschau fertig", b: "Ausbreitung abgeschätzt", c: "Alle Kräfte im Gleichgewicht"}, a: ["c"] },
        { id: 33, q: "33 Welche Einrichtungen können als Rettungsweg dienen?", o: {a: "Treppen", b: "Tragbare Leitern", c: "Fluchthauben", d: "Aufzüge", e: "Drehleitern"}, a: ["a", "b", "e"] },
        { id: 34, q: "34 Wie lautet die bauaufsichtliche Benennung für F 30-B?", o: {a: "Feuerbeständig", b: "Feuerhemmend", c: "Nicht brennbar", d: "Schwer entflammbar"}, a: ["b"] },
        { id: 35, q: "35 Was sind lebensrettende Sofortmaßnahmen?", o: {a: "Verkehrssicherung", b: "Schienen von Brüchen", c: "Wiederbelebung und Atemspende", d: "Lagerung und Schockbekämpfung", e: "Anschrift notieren"}, a: ["c", "d"] },
        { id: 36, q: "36 Was muss zur Aufrechterhaltung oder Wiederherstellung der Vitalfunktionen getan werden?", o: {a: "Blutstillung", b: "Warmes Getränk", c: "Injektion", d: "Lagerung", e: "Wärmeerhaltung", f: "Puls und Blutdruck prüfen", g: "Atmung und Bewusstsein prüfen", h: "Notruf", i: "Befragen"}, a: ["a", "d", "e", "f", "g"] },
        { id: 37, q: "37 Welches sind Anzeigen die auf einen Schock hindeuten?", o: {a: "Durst", b: "Kopfschmerz", c: "Frieren und Zittern", d: "Blasse, kalte und feuchte Haut", e: "Starke Blutungen", f: "Unruhe", g: "Schwacher Puls", h: "Tiefer Schlaf", i: "Heiterkeit", j: "Mitteilungsbedürfnis"}, a: ["c", "d", "e", "f", "g"] },
        { id: 38, q: "38 Wo befindet sich die UTM-Koordinate 372378?", o: {a: "Stelle A", b: "Stelle B"}, a: ["a"] },
        { id: 39, q: "39 Was ist beim Retten aus Schächten zu beachten?", o: {a: "Sauerstoff anreichern", b: "Atemschutz und Leine", c: "Leine und Gurt sichern"}, a: ["b", "c"] },
        { id: 40, q: "40 Warum werden Feuerwehr-Einsatzpläne erstellt?", o: {a: "Bürgermeister informieren", b: "Einsatzleiter über Gefahren informieren", c: "KBM Leitung übernehmen"}, a: ["b"] },
        { id: 41, q: "41 Welches sind die ersten Tätigkeiten am Gerätehaus?", o: {a: "Motor warmlaufen lassen", b: "Ausrüstung prüfen", c: "Verbindung zur Leitstelle herstellen", d: "KBM verständigen", e: "Auftrag erfragen", f: "Auftrag wiederholen und notieren"}, a: ["c", "e", "f"] },
        { id: 42, q: "42 Wie wird die Unterstützung anderer Feuerwehren angefordert?", o: {a: "Über Telefon", b: "Über die Leitstelle", c: "Direkt über Funk"}, a: ["b"] },
        { id: 43, q: "43 Welche Begriffe müssen im Befehl enthalten sein?", o: {a: "Einheit", b: "Lage", c: "Hydranten", d: "Auftrag", e: "Ort und Zeit", f: "Mittel", g: "Ziel", h: "Weg", i: "Schadenstelle"}, a: ["a", "d", "f", "g", "h"] },
        { id: 44, q: "44 Wer entscheidet über medizinische Maßnahmen?", o: {a: "TEL", b: "Bürgermeister", c: "Werksdirektor", d: "Notarzt", e: "Kommandant", f: "Sicherheitsbeauftragte"}, a: ["d"] },
        { id: 45, q: "45 Welches sind die Grundsätze im Sprechfunkverkehr?", o: {a: "Höflichkeitsformeln unterlassen", b: "Namen und Amt weglassen", c: "Teilnehmer Siezen", d: "Funkdisziplin halten", e: "Abkürzungen verwenden", f: "Buchstabieren schwieriger Wörter", g: "Zahlen deutlich sprechen", h: "Laut ohne Pause sprechen"}, a: ["a", "c", "d", "f", "g"] },
        { id: 46, q: "46 Wie werden die Verkehrsarten unterschieden?", o: {a: "Wechselverkehr", b: "Richtungsverkehr", c: "Kreisverkehr", d: "Sternverkehr", e: "Gegenverkehr", f: "Bedingter Gegenverkehr", g: "Linienverkehr"}, a: ["a", "b", "e", "f"] },
        { id: 47, q: "47 Welche der folgenden Aussage ist richtig?", o: {a: "Einfachnachrichten nach Eingang", b: "Sofortnachrichten vor Einfachnachrichten", c: "Gespräche vorfertigen", d: "Ein Gespräch ist formlos", e: "Ein Spruch ist formgebunden und schriftlich"}, a: ["a", "b", "d", "e"] },
        { id: 48, q: "48 Was bedeutet auf der Warntafel die Gefahrnummer 856?", o: {a: "Ätzend, brandfördernd und giftig", b: "Giftig, brandfördernd und ätzend", c: "Oxidierend, giftig und ätzend"}, a: ["a"] },
        { id: 49, q: "49 Was bedeutet auf der Warntafel die Gefahrnummer X 423?", o: {a: "Fester Stoff, reagiert mit Wasser (brennbare Gase)", b: "Flüssiger Stoff, reagiert mit Wasser", c: "Fester Stoff, leicht entzündbar"}, a: ["a"] },
        { id: 50, q: "50 Was bedeutet beim Hommel Gefahrendiamant die blaue 3?", o: {a: "Reaktion", b: "Vollschutzanzug erforderlich", c: "30m Zone", d: "Schwer entflammbar", e: "Gesundheitsgefahr"}, a: ["b", "e"] },
        { id: 51, q: "51 Gaswolke im Freien - Was ist die erste Maßnahme zum Schutz der Bevölkerung?", o: {a: "Räumung", b: "Abdichten von Häusern", c: "Niederschlagen mit Wassernebel"}, a: ["c"] },
        { id: 52, q: "52 VU mit ätzenden Behältern - Welche Maßnahmen?", o: {a: "50m absichern", b: "Gase mit Wassernebel niederschlagen (AS)", c: "GW-G / KBM anfordern", d: "RW / Transportfirma", e: "In Kanal spülen"}, a: ["a", "b", "c"] },
        { id: 53, q: "53 Was ist ein Gefährlichkeitsmerkmal von Stoffen?", o: {a: "Sehr giftig", b: "Ekelhaft", c: "Brandfördernd", d: "Krebserzeugend", e: "Explosionsgefährlich"}, a: ["a", "c", "d", "e"] },
        { id: 54, q: "54 Woran erkennt man giftige Stoffe?", o: {a: "Nicht ohne weiteres", b: "Mit Prüfröhrchen", c: "Mit pH-Papier"}, a: ["a"] },
        { id: 55, q: "55 Welche Aussagen zu Dekon sind richtig?", o: {a: "Innerhalb 15 min bereit", b: "Stufe I Not-Dekon", c: "Stufe II Standard-Dekon", d: "Stufe III Erweiterte-Dekon", e: "Platzbedarf bei Gruppe II/III beachten"}, a: ["a", "b", "c", "d", "e"] },
        { id: 56, q: "56 Was bedeutet die Abkürzung TRGS?", o: {a: "Technische Regeln für Gefahrstoffe", b: "Transport-Richtlinie", c: "Regelwerk für Gase"}, a: ["a"] },
        { id: 57, q: "57 Welche Aussage für Erdgas ist richtig?", o: {a: "Leichter als Luft", b: "Gut riechbar", c: "Wirkt als Blutgift"}, a: ["a"] },
        { id: 58, q: "58 Welche Schutzmaßnahmen bei Radioaktivität gibt es?", o: {a: "Abstand halten", b: "Aufenthaltszeit kurz halten", c: "Abschirmung nutzen", d: "Kontamination vermeiden", e: "Inkorporation vermeiden", f: "Stoffe kühlen", g: "Stoffe aufnehmen"}, a: ["a", "b", "c", "d", "e"] },
        { id: 59, q: "59 Wie groß ist die Absperrgrenze bei Radioaktivität?", o: {a: "10 Meter", b: "25 Meter", c: "50 Meter"}, a: ["c"] },
        { id: 60, q: "60 Wie erfolgt die Zuordnung zu den Gefahrengruppen ABC?", o: {a: "Gruppe I mit Sonderausrüstung", b: "Gruppe II ohne Sonderausrüstung", c: "Gruppe III mit Sonderausrüstung und Fachkundigem"}, a: ["c"] }
    ];

    let currentQuestions = [];
    let wrongQuestions = [];
    let currentIdx = 0;
    let score = 0;

    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
    }

    function startQuiz(questionsToUse) {
        currentQuestions = [...questionsToUse];
        shuffle(currentQuestions);
        currentIdx = 0;
        score = 0;
        wrongQuestions = [];
        displayQuestion();
    }

    function displayQuestion() {
        const item = currentQuestions[currentIdx];
        document.getElementById("progress").innerText = `Frage ${currentIdx + 1} von ${currentQuestions.length}`;
        document.getElementById("question-display").innerText = item.q;
        const optionsDiv = document.getElementById("options-display");
        optionsDiv.innerHTML = "";
        
        for (const [key, value] of Object.entries(item.o)) {
            const label = document.createElement("label");
            label.className = "option";
            label.innerHTML = `<input type="checkbox" value="${key}"> ${key}) ${value}`;
            label.onclick = function(e) {
                if(e.target.tagName !== "INPUT") {
                    const cb = this.querySelector("input");
                    cb.checked = !cb.checked;
                }
                this.classList.toggle("selected", this.querySelector("input").checked);
            };
            optionsDiv.appendChild(label);
        }
        
        document.getElementById("feedback").style.display = "none";
        document.getElementById("next-btn").style.display = "none";
        document.getElementById("override-btn").style.display = "none";
        document.getElementById("check-btn").style.display = "block";
    }

    function checkAnswer() {
        const item = currentQuestions[currentIdx];
        const selected = Array.from(document.querySelectorAll("#options-display input:checked")).map(i => i.value);
        const feedback = document.getElementById("feedback");
        
        const isCorrect = JSON.stringify(selected.sort()) === JSON.stringify(item.a.sort());
        
        feedback.style.display = "block";
        if (isCorrect) {
            feedback.className = "correct";
            feedback.innerText = "RICHTIG!";
            score++;
            document.getElementById("override-btn").style.display = "none";
        } else {
            feedback.className = "wrong";
            feedback.innerText = "FALSCH! Richtige Lösung: " + item.a.join(", ").toUpperCase();
            wrongQuestions.push(item);
            document.getElementById("override-btn").style.display = "block";
        }
        
        document.getElementById("check-btn").style.display = "none";
        document.getElementById("next-btn").style.display = "block";
    }

    function overrideCorrect() {
        const item = currentQuestions[currentIdx];
        wrongQuestions = wrongQuestions.filter(q => q.id !== item.id);
        score++;
        const feedback = document.getElementById("feedback");
        feedback.className = "correct";
        feedback.innerText = "Als RICHTIG gewertet!";
        document.getElementById("override-btn").style.display = "none";
    }

    function nextQuestion() {
        currentIdx++;
        if (currentIdx < currentQuestions.length) {
            displayQuestion();
        } else {
            showResults();
        }
    }

    function showResults() {
        const percent = Math.round((score / currentQuestions.length) * 100);
        let resultHTML = `<div class="stats"><h2>Ergebnis</h2>`;
        resultHTML += `<div class="percent-box">${percent}%</div>`;
        resultHTML += `<p>Du bist zu ${percent}% vorbereitet.</p>`;
        resultHTML += `<p>Korrekt: ${score} von ${currentQuestions.length}</p>`;
        
        if (wrongQuestions.length > 0) {
            resultHTML += `<button onclick="retryWrong()">Falsche Fragen wiederholen (${wrongQuestions.length})</button>`;
        }
        resultHTML += `<button style="background:#666;" onclick="location.reload()">Alles neu starten</button></div>`;
        
        document.getElementById("quiz-box").innerHTML = resultHTML;
        document.getElementById("progress").innerText = "Training beendet";
    }

    function retryWrong() {
        document.getElementById("quiz-box").innerHTML = `
            <span class="question-text" id="question-display"></span>
            <div id="options-display"></div>
            <button id="check-btn" onclick="checkAnswer()">Antwort prüfen</button>
            <div id="feedback"></div>
            <button id="override-btn" onclick="overrideCorrect()">Trotzdem als RICHTIG werten</button>
            <button id="next-btn" onclick="nextQuestion()">Nächste Frage</button>
        `;
        startQuiz(wrongQuestions);
    }

    startQuiz(allQuestions);
</script>

</body>
</html>
