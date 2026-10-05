# Startseite nach Figma

Die Startseite (`src/pages/index.astro`) setzt den Rahmen der Hauptseite (Node `4:640`) aus der
[Figma-Datei zur aktiVaria-Website](https://www.figma.com/design/B0cc8tw2hIAQk5Y2G1tdsG/), Seite "Screens (aus XD)", um.
Maßgeblich ist der Desktop-Entwurf mit 1920 px Breite. Für Tablet und Smartphone gelten die Rahmen
"Hauptseite Tablet 768" (`173:263`) und "Hauptseite Mobil 390" (`173:327`) sowie die Notizen auf der Seite
"Design System".

## Aufbau

| Figma-Rahmen | Komponente |
|---|---|
| 01 Navigation | `src/components/Header.astro`, `AudienceSwitch.astro` |
| 02 Hero | `src/components/home/Hero.astro` |
| 03 Kategorien | `src/components/home/Kategorien.astro`, `OfferTile.astro` |
| 04 Was uns bewegt + Momente | `src/components/home/WasUnsBewegt.astro`, `SliderIndicator.astro` |
| 05 Vertrauen & Partnerlogos | `src/components/home/Vertrauen.astro` |
| 06 Team | `src/components/home/Team.astro` |
| 07 Qualifikationen | `src/components/home/Qualifikationen.astro`, `QualificationCard.astro` |
| 08 Footer | `src/components/Footer.astro` |

Farben, Radien und Textstile stammen aus der Variablensammlung "aktiVaria Tokens" und stehen in
`src/styles/global.css`. Aus `color/brand/blue-700` wird `--color-brand-blue-700`, aus `text/body-lg`
die Klasse `text-body-lg`.

## Hintergründe

Die Hintergründe sind in Figma mehrere übereinanderliegende Ebenen mit Füllmethoden. Im Code sind sie
genauso aufgebaut, damit Text echter Text bleibt:

- `texture-*.webp`: die helle Papierstruktur (`img/hero-1`), je Abschnitt zugeschnitten.
- Verläufe `art/hero-3`, `art/kategorien-gesundheit-un-1`, `art/footer-1`, `art/footer-8`: als CSS-Verlauf
  mit `mix-blend-mode: multiply` (`.gradient-*` in `global.css`).
- `kategorien-bubbles.webp`: Blasen mit `mix-blend-mode: screen`. Sie reichen wie in Figma 125 px in den
  Hero hinein. Die Sektion darf deshalb keinen eigenen Stapelkontext bekommen (kein `isolate`, kein `z-index`).
- `was-uns-bewegt-wave.webp`: Wellenbild mit `mix-blend-mode: multiply`.

Die Rasterbilder sind 1:1-Renderings der jeweiligen Figma-Ebene, die Zierblasen (`public/images/decor`)
SVG-Exporte. Die 18 Fotos des Laufbands (`public/images/home/momente`) sind einzeln in 1,5-facher Auflösung
gerendert, mit der Blob-Maske als Transparenz.

In vier der Zierblasen-SVGs sind Verläufe ohne Länge durch Weiß ersetzt, weil Figma solche Verläufe mit dem
ersten Farbstopp (Weiß) zeichnet, Browser aber mit dem letzten (Blau).

## Bewusste Abweichungen vom Entwurf

- "PROFESSIONELL" und "INDIVIDUELL" brechen in Figma mitten im Wort um, weil das Textfeld zu schmal ist.
  Im Code stehen sie in einer Zeile.
- Im Footer liegt in Figma der zweite Verlauf über Überschrift, Mitgliedschaften, Logo, Social-Icons und
  Copyright und dunkelt sie ab. Das ist eine Folge der Ebenenreihenfolge aus dem XD-Import. Im Code ist der
  gesamte Footer-Inhalt weiß, wie es der Design-System-Hinweis zum Footer-Link vorsieht.
- Rettungsschwimmabzeichen: "und ist für das Erkennen" ist zu "und sind für das Erkennen" korrigiert.
- "Mitarbeitende" heißt im Code "Mitarbeiter" (Kennzahlen, Ausbildereignung). Auf der Website wird nicht gegendert.
- Bildstreifen "Momente, die bewegen" (alle 18 Fotos aus Figma) und Partnerlogos laufen als endlose
  Laufbänder (`src/components/Laufband.astro`). Beim Start entsprechen sie dem Entwurf, beide mit rund
  40 px pro Sekunde. Sie halten beim Überfahren mit der Maus, bei Tastaturfokus und beim Antippen an und
  stehen still, wenn im System "Bewegung reduzieren" eingestellt ist. Die Anzeige darunter folgt dem
  Durchlauf, ein Klick springt an die Stelle.
- Roboto wird selbst ausgeliefert (`@fontsource/roboto`) statt von Google Fonts geladen.
