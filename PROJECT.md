# PROJECT.md — Music Sandbox (Retro Analog DAW)

> **Täytä tämä tiedosto projektin alussa. Päivitä, kun tekniset valinnat muuttuvat.**
> Tämä tiedosto luetaan yhdessä `AGENTS.md`:n kanssa.  
> **Tämän projektin virallinen AGENTS.md:** `/home/workdir/artifacts/MUSIC-SANDBOX-AGENTS.md`

---

## 1. Visio

**Mitä tämä sovellus tekee?**
Selainpohjainen, täysin offline, single-file retro-analog DAW (Digital Audio Workstation).  
Käyttäjä voi luoda musiikkia Impulse Tracker -tyylisellä 8-kanavaisella sequencerilla, 8-pad rumpukoneella, melodialla, reaaliaikaisella mikillä ja per-kanava efekteillä — kaikki yhdessä lämpimässä analogisessa käyttöliittymässä. Mixer on aivot. Kaikki ääni kulkee sen kautta.

**Kenelle?**
Pääasiassa itselleni (ADHD-luova tekijä, joka tekee 3D:tä, musaa, pelejä, opetusta).  
Mahdollisesti jaettavaksi muille samanhenkisille (pieni ryhmä, ei massamarkkinat).

**Millä laitteella käytetään?**
- Pääasiassa läppäri (Chrome / Firefox / Safari)
- Toissijaisesti puhelin / tabletti (iOS Safari, Android Chrome)
- Ei TV:tä tai vanhoja selaimia.

**Mikä tekee tästä unohtumattoman?**
Lämpimät putkien hehku-animaatiot + Orange Amps -tyylinen analoginen ote.  
Kun kytket efektin päälle, putki “lämpenee” kauniisti 1.2 sekunnissa — se tuntuu oikealta vintage-mikseriltä. Tämä pieni detalji tekee musiikinteosta hauskaa, taktiilista ja dopamiinipitoista ilman että käyttöliittymä vie huomiota pois itse luomisesta.

---

## 2. Visuaalinen suunta

**Tunnelma / tyyli:**
Kokoelma aitoja vintage hardware -yksiköitä — jokaisella omat persoonallinen brändi, look, logo ja fiilis.  
Ei yksi yhtenäinen teema, vaan “studiollinen oikeita laitteita” rinnakkain.

**Esimerkit (tämä on suunta, ei lopullinen):**
- **Mixer** — Toft-style British console (tumma vihreä / walnut + cream, puhdas, professional, metalliset nupit, selkeä kanavastrip)
- **Tape Delay** — Oma vintage reel-to-reel look (lämmin ruskea, kulunut nauha, pyörivät kelat, “worn tape” -teksttuuri)
- **Compressor** — LA-2A / 1176 -tyylinen (musta etupaneeli, iso toimiva VU-mittari, vain muutama säädin, klassinen “gain reduction” -neula)
- Muut efektit saavat omat persoonalliset ilmeet (esim. Spring Reverb = vanha kitara-ampin reverb tankki, Distortion = pedal-style musta box, Chorus = 80-luvun rack unit jne.)

Käyttäjä tekee / hioo lopulliset tekstuurit, logot, kaiverrukset ja hienosäädöt myöhemmin.  
Tällä hetkellä käytetään kevyitä CSS + SVG -placeholderit, jotka on helppo korvata oikeilla grafiikoilla ilman että rakennetta tarvitsee muuttaa.

**Tärkeimmät värit (kokonaisuus):**
Lämmin analoginen paletti (ei yhtä oranssia). Jokainen moduuli määrittää omat värinsä, mutta kokonaisuus pysyy lämpimänä ja studiomaisena.

**Typografia:**
- Kaikki: system-ui mono (`ui-monospace`, "SF Mono", "Cascadia Mono")
- Otsikot: `VT323` tai `Press Start 2P` (vintage arcade-fiilis)
- Numerot: tabular-nums + font-variant

**Erityiset visuaaliset elementit:**
- Moduulikohtaiset 3D-knobit ja mittarit (pyörivät, metallinen reuna)
- Putkien / lamppujen hehku (kun efekti on päällä)
- Toimivat VU-mittarit (neula tai LED-palkki)
- Subtle scanline + noise overlay + CRT vignette (tausta)
- Hover-lift + soft halo kaikille kontrolleille
- Power-on cascade (moduulit lämpenevät yksi kerrallaan)

**Tavoite:** Jokainen widget tuntuu omalta pieneltä vintage-laitteelta, jolla on oma persoona ja brändi.

---

## 3. Tekniset valinnat (ja niiden perustelut)

**Pohjastack:**
- [x] Vanilla HTML5 + CSS3 + JavaScript (ES2023)
- [x] PWA (manifest + mahdollinen service worker myöhemmin)
- [ ] Ei frameworkeja (React/Vue/Svelte kielletty ilman vahvaa perustelua)

**Kirjastot (perustelut):**

| Kirjasto     | Mihin tarkoitukseen          | Miksi tämä, ei vanilla |
|--------------|------------------------------|------------------------|
| Tone.js      | Transport + BPM-synkronointi | Web Audio API:n setTimeout/setInterval aiheuttaa drift; Tone.js on de-facto standardi selain-DAW:eissa (GridSound, openDAW) ja ratkaisee ongelman ilman että itse keksitään pyörää |
| Web Audio API| Kaikki äänenkäsittely        | Natiivi, ei riippuvuuksia, täysi kontrolli |

**Frameworkit:** Ei käytetä. Perustelu: sovelluksen monimutkaisuus on hallittavissa vanillalla + Web Audio API:lla. Framework lisäisi vain turhaa kompleksisuutta ja bundle-kokoa.

**Datan tallennus:**
- [x] localStorage — patterns, BPM, channel states, presets (alle 5 MB)
- [ ] IndexedDB — ei vielä tarvita (patterns mahtuvat localStorageen)
- [ ] File System Access API — myöhemmin (pattern export/import)

**Käytetyt Web API:t:**
- Web Audio API (AudioContext, OscillatorNode, GainNode, BiquadFilterNode, MediaRecorder, MediaStream)
- Canvas 2D (tracker grid, drum sequencer)
- localStorage
- requestAnimationFrame + performance.now() ajoitukseen
- Fullscreen API (mahdollinen)
- Vibration API (mobile feedback)

**Perustelu kaikille:** Kaikki natiiveja, tuettu kaikilla kohdelaitteilla, ei ulkoisia riippuvuuksia paitsi Tone.js (pieni ja perusteltu).

---

## 4. Kohdelaitteet ja yhteensopivuus

**Pakolliset (testattava ennen jokaista releasea):**
- [x] Desktop Chrome (viimeiset 2 versiota)
- [x] Desktop Firefox (viimeiset 2 versiota)
- [x] Desktop Safari (viimeiset 2 versiota)
- [x] iOS Safari (viimeiset 2 versiota)
- [x] Android Chrome (viimeiset 2 versiota)

**Ei tarvitse tukea:**
- IE, vanhat Android < 10, TV-selaimet, vanhat iOS < 15

**Erityiset näyttökoot:**
- Optimoitu 1280–1920 px (läppäri) + 375–768 px (puhelin/tabletti)
- Container queries + media queries

---

## 5. Erityisvaatimukset

**Pakollinen offline-toiminta:**
- [x] Kyllä, 100 % ydintoiminnallisuus (kaikki audio, tallennus, patternit)

**Asennettavuus (PWA):**
- [x] Kyllä, oma ikoni + manifest (lisätään v0.9)

**Suorituskykytarvikkeet:**
- Lighthouse Performance ≥ 90
- 60 fps animaatiot (tube glow, knob rotation, VU)
- JS-koko < 250 KB pakkaamattomana (Tone.js mukaan lukien)

**Saavutettavuus:**
- WCAG AA kontrasti
- Näppäimistönavigointi (Tab, Space, nuolet, 1–8 trackerille)
- `prefers-reduced-motion` → poistaa tube warm-up ja glow-animaatiot

---

## 6. Versiohistorian pääpisteet

| Versio | Päivä       | Iso muutos |
|--------|-------------|------------|
| v0.1   | 2026-04-26  | Ensimmäinen prototyyppi (perus tracker + mixer) |
| v0.2   | 2026-04-27  | Mixer aivoina + insert + aux-sends |
| v0.3   | 2026-04-27  | Bugikorjaukset + parempi tracker note input |
| v0.4   | 2026-04-27  | Retro analog + per-channel ON/OFF tube switches (ei drag & drop) |
| v0.5   | 2026-04-27  | PIXELFORGE: Orange tolex, 3D knobs, tube warm-up, VU meters, micro-animations |
| v0.6   | Planned     | Convolution reverb (ConvolverNode + impulse) |
| v0.7   | Planned     | Parempi synth engine + sampler trackerille |
| v0.8   | Planned     | .wav export + pattern save/load |
| v0.9   | Planned     | Mobile touch + PWA manifest |
| v1.0   | Goal        | Julkinen release + täysi AGENTS.md v1.0 |

(Yksityiskohtainen historia: `CHANGELOG.md`)

---

## 7. Avoimet kysymykset / tehtävät

- [ ] Pitääkö trackerissa olla oikeita sampleja vai riittääkö generative synth?
- [ ] Halutaanko aux-sends takaisin myöhemmin vai pysytäänkö tiukasti per-channel insert + ON/OFF?
- [ ] Miten patternit tallennetaan/puretaan (JSON vs .it-tiedostot)?
- [ ] Halutaanko 4-kanavainen “lite” versio mobiilille?

---

## 8. Inspiraatio ja referenssit

**Visuaalinen:**
- Orange Amps (OR15, Rockerverb, AD30) — tolex, nupit, lämpö
- GridSound DAW (gridsound/daw) — puhdas single-file Web Audio DAW
- openDAW (andremichelle/openDAW) — modulaarinen rakenne
- 80s Retro Interfaces (Behance) — vintage panel design
- Travis Geis Knobs — klassinen analog nuppi referenssi

**Tekninen:**
- audiojs/web-audio-api/AGENTS.md — pakollinen lukeminen kaikille DSP-muutoksille
- MDN Web Audio Best Practices
- GridSound source code — erinomainen esimerkki puhtaasta rakenteesta

**Musiikillinen:**
- Impulse Tracker (Amiga) — tracker-fiilis
- Reason 2 — mixer + channel strip -estetiikka
- Old-school hardware synths & mixers — analoginen sielu

---

**Tämä PROJECT.md on nyt ajan tasalla v0.5 (PIXELFORGE) ja linjassa MUSIC-SANDBOX-AGENTS.md:n kanssa.**

Seuraava agentti: Lue tämä + AGENTS.md ennen kuin kosket koodiin. Kaikki päätökset dokumentoidaan tänne.