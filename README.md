
# EBE POWER — strona wizytówka

Strona prezentacyjna (wizytówka) dla firmy **EBE POWER** zajmującej się sprzedażą agregatów
prądotwórczych. Ciemny, industrialny layout: grafit + żółty akcent, kondensacyjne nagłówki,
duże zdjęcia, animacje przy przewijaniu.

**Stack:** React 19 + Vite + TypeScript + Tailwind CSS v4 + React Router.

## Uruchomienie

```bash
npm install
npm run dev      # http://localhost:5173
npm run build    # produkcyjny build do katalogu dist/
npm run preview  # podgląd zbudowanej wersji
```

## Wdrożenie na Cloud Run

Repozytorium zawiera `Dockerfile` (nginx + statyczne pliki z `dist/`) oraz
skrypt `start` (Vite preview) — oba nasłuchują na porcie z zmiennej `PORT`
(Cloud Run ustawia `PORT=8080`).

```bash
gcloud run deploy wizytowka --source . --region europe-central2 \
  --allow-unauthenticated
```

albo przez Cloud Build trigger na tym repozytorium (wykryje `Dockerfile`
automatycznie). Za pierwszym razem wybierz region i potwierdź utworzenie usługi.

## Struktura

```
src/
├─ content/
│  └─ site.ts          ← CAŁA TREŚĆ I DANE FIRMY (teksty, oferta, kontakt, ceny, marki, FAQ)
├─ components/         ← Navbar, Footer, Hero, kalkulator, formularz, galeria, FAQ…
├─ pages/              ← Home, Oferta, Realizacje, O firmie, Kontakt, Polityka prywatności, 404
├─ App.tsx             ← trasy (react-router)
└─ index.css           ← system wizualny (kolory, kroje, animacje, utility)
public/images/         ← zdjęcia (wygenerowane, gotowe do podmiany na własne)
```

Podstrony: `/` · `/oferta` · `/realizacje` · `/o-firmie` · `/kontakt` · `/polityka-prywatnosci` · 404.

## ⚠️ Przed wdrożeniem — lista rzeczy do zrobienia

1. **Dane firmy** — w pliku `src/content/site.ts` zostały już wpisane: telefon `888 883 232`,
   e-mail `kontakt@ebe-power.pl`, adres `Borki 10, 97-400 Bełchatów`, NIP `769 183 05 28`,
   godziny `Pn–Pt 8:00–16:00`. Do uzupełnienia pozostały pola oznaczone `TODO`:
   pełna nazwa prawna (forma działalności), REGON/KRS (jeśli dotyczą), numer konta,
   osobny numer serwisowy, rok założenia oraz linki do social mediów.
   Pola puste są automatycznie ukrywane na stronie.
2. **Formularz kontaktowy** — domyślnie otwiera program pocztowy (mailto). Aby działał bez
   poczty, ustaw w `site.ts` pole `formEndpoint` (np. Formspree) lub pod własny endpoint.
   Zmienną środowiskową: `VITE_FORM_ENDPOINT` w pliku `.env`.
3. **Zdjęcia** — obrazy w `public/images/` są wygenerowane i mają charakter poglądowy.
   Podmień je na zdjęcia własnych agregatów i realizacji (zachowaj nazwy plików lub zaktualizuj
   ścieżki w `site.ts`).
4. **Treść oferty i realizacje** — opisy, zakresy mocy, ceny „od”, marki oraz projekty w
   galerii to propozycja do zatwierdzenia/poprawy (w `site.ts`).
5. **Domena i SEO** — ustaw w `index.html` pełne adresy w tagach `og:image`, dodaj
   `sitemap.xml`, `robots.txt` oraz prawdziwy adres kanoniczny (komponent `Seo` ustawia
   canonical z `window.location.origin`).
6. **Polityka prywatności / RODO** — tekst w `src/pages/Privacy.tsx` jest szablonem;
   przed uruchomieniem formularza zastąp go dokumentem firmowym.

## Najważniejsze elementy

- **Kalkulator doboru mocy** (`/oferta#kalkulator`) — użytkownik zaznacza odbiorniki, wpisuje
  ich moc, ustawia jednoczesność i rezerwę; wynik to wymagana moc w kVA, rekomendowana
  standardowa wielkość agregatu i ostrzeżenie o niedociążeniu (< 30%). Wynik można przenieść
  do formularza na `/kontakt` jednym kliknięciem.
- **Formularz kontaktowy** — walidacja, zgoda RODO, obsługa błędów, wysyłka przez endpoint
  lub mailto, ekran potwierdzenia.
- **Galeria realizacji** z filtrem kategorii i podglądem zdjęcia.
- **FAQ** (rozwijane), liczniki animowane, stopka z danymi rejestrowymi, dane
  strukturalne `LocalBusiness` (schema.org), meta tagi OG per podstrona.
- Dostępność: skip-link, focus-visible, `prefers-reduced-motion`, semantyczne nagłówki.

