# Earlynest – wiedza o projekcie

Ten plik jest źródłem prawdy o projekcie. Aktualizuj go, gdy coś ustalimy lub zmienimy.
Język rozmowy z właścicielem: polski.

## Czym jest Earlynest

- Aplikacja mobilna (iOS i Android) dla rodziców, których dzieci chodzą do żłobków i przedszkoli.
- Panel webowy dla placówek (żłobków/przedszkoli) do zarządzania.
- **Problem, który rozwiązujemy:** wiele placówek nie ma żadnego systemu komunikacji z rodzicami, więc rodzice muszą dzwonić.

## Odbiorcy (dwie grupy, różne potrzeby)

1. **Rodzice** – korzystają z aplikacji mobilnej (tej samej, z której może korzystać personel). Szukają np. „aplikacja dla rodziców przedszkole", „jak zgłosić nieobecność dziecka w żłobku".
2. **Placówki** – korzystają z panelu webowego, a ich personel także z aplikacji mobilnej. Szukają np. „system do zarządzania przedszkolem", „program dla żłobka", „komunikacja z rodzicami".

## Aktualny stan

- Repozytorium jest na razie puste (brak kodu).
- Etap obecny: ustalanie wiedzy i strategii. **Nic nie kodujemy, dopóki właściciel o to nie poprosi.**

## Wymóg: produkt globalny i wielojęzyczny

Aplikacja może być używana na całym świecie, więc wielojęzyczność dotyczy **całego produktu**, a nie tylko strony:

- **Aplikacja mobilna (iOS/Android)** i **panel webowy** od początku budowane z i18n: żadnych tekstów na sztywno w kodzie, wszystkie w plikach tłumaczeń.
- Architektura ma wspierać **dowolną liczbę języków**; dodanie języka = dodanie plików tłumaczeń, bez zmian w kodzie.
- Język użytkownika wybierany z ustawień telefonu/przeglądarki, z możliwością ręcznej zmiany w aplikacji.
- **Placówka i rodzic mogą mieć różne języki**; wiadomości i powiadomienia powinny być tłumaczone lub dostarczane w języku odbiorcy (do ustalenia, czy tłumaczenie maszynowe).
- Wsparcie dla pisma **od prawej do lewej (RTL)**, np. arabski, hebrajski – układ interfejsu musi to uwzględniać.
- Lokalne formaty: daty, godziny, liczby, waluty, strefy czasowe, pierwszy dzień tygodnia, formaty imion i adresów.
- Terminologia: żłobek/przedszkole/szkółka mają inne znaczenie i wiek dzieci w różnych krajach – słownik pojęć per kraj.
- Prawo: RODO (UE) to minimum; inne rynki mają własne przepisy o danych dzieci (np. COPPA w USA). Sprawdzić przed wejściem na rynek.
- Sklepy z aplikacjami: pełna lokalizacja w każdym wspieranym języku (patrz ASO niżej).

## Wymóg: prosta architektura, jedna aplikacja dla wszystkich

- **Jedna aplikacja mobilna** (jedna na iOS, jedna na Android) dla wszystkich rodziców, placówek i krajów. Jeden wpis w każdym sklepie, jeden build.
- **Bez wersji white-label** i bez osobnych aplikacji per placówka, per kraj czy per język.
- Przynależność do placówki to **dane, a nie osobna aplikacja**: rodzic dołącza do placówki przez link, kod QR lub kod zaproszenia (patrz „Wyszukiwanie lokalne") i jedna aplikacja pokazuje jej dane. Jeden rodzic może mieć wiele dzieci i wiele placówek.
- Język, region i terminologia to ustawienia konfiguracji/danych, a nie osobne wersje kodu.
- Zasada: **najprostsze rozwiązanie, które działa**. Każda dodatkowa warstwa, usługa lub wariant aplikacji wymaga uzasadnienia. Nie budujemy na zapas.
- **Personel placówki także korzysta z aplikacji mobilnej** (obok panelu webowego). Czyli ta sama aplikacja obsługuje rodziców i personel.
- **Role zamiast osobnych aplikacji:** jedno konto użytkownika może mieć różne role w różnych placówkach (np. rodzic w jednej, opiekun w innej). Aplikacja pokazuje widok i funkcje zależnie od roli i placówki. Uprawnienia egzekwuje backend, a nie tylko interfejs.
- **Model pojęć (do doprecyzowania przy projektowaniu danych):**
  - **Użytkownik** (konto): rodzic, personel lub właściciel; jedno konto, wiele ról.
  - **Dziecko**: może być powiązane z wieloma opiekunami (np. dwoje rodziców) i należeć do jednej placówki oraz jednej grupy.
  - **Placówka**: żłobek/przedszkole, ma grupy.
  - **Grupa**: w obrębie placówki; rodzeństwo w tej samej placówce może być w różnych grupach.
  - **Organizacja/właściciel**: placówki komercyjne mogą należeć do jednego właściciela, który ma **wiele placówek** i zarządza nimi z jednego konta.
- **Łatwe przełączanie kontekstu w aplikacji (kluczowy wymóg UX):**
  - Rodzic może mieć **wiele dzieci w jednej placówce** (nie zawsze w tej samej grupie) oraz **dzieci w różnych placówkach**.
  - Aplikacja musi pozwalać jednym ruchem przełączyć się między dzieckiem i placówką (np. selektor na górze ekranu), bez wylogowania i bez osobnych kont.
  - Oprócz widoku pojedynczego dziecka jest widok zbiorczy „wszystkie dzieci" (patrz niżej).
  - Powiadomienia mają jasno wskazywać, którego dziecka i której placówki dotyczą.
  - Ten sam mechanizm dotyczy personelu pracującego w kilku placówkach oraz właściciela z wieloma placówkami (przełączanie placówki lub widok zbiorczy całej organizacji).
- **Widok zbiorczy (ustalone):** rodzic i właściciel organizacji mają widok zbiorczy, np. wszystkie nieobecności ze wszystkich dzieci/placówek w jednym miejscu, obok przełączania na pojedyncze dziecko lub placówkę.
- **Historia dziecka (ustalone):** dziecko może zmieniać grupę, a nawet placówkę. Zapisujemy historię przynależności (placówka, grupa, daty od–do), a nie nadpisujemy danych. Dziecko zmieniające placówkę nie traci powiązań z opiekunami, ale dane z poprzedniej placówki pozostają w niej (granice dostępu do ustalenia, patrz RODO).
- **Opiekunowie (ustalone):** liczba opiekunów jednego dziecka jest **dowolna, bez limitu**. Model ma być generyczny: opiekunem może być rodzic, dziadek, opiekun prawny itd., także zamiast rodziców lub razem z nimi.
  - Powiązanie opiekun–dziecko to osobny byt z własnymi danymi: rodzaj relacji (np. rodzic, dziadek, opiekun prawny, inna osoba), status (oczekujący, zatwierdzony, odrzucony, wygasły/odebrany), daty od–do oraz uprawnienia (np. odbieranie dziecka, otrzymywanie powiadomień, zgłaszanie nieobecności).
  - **Placówka zatwierdza nowego opiekuna.** Nowy opiekun nie widzi danych dziecka, dopóki placówka go nie zatwierdzi. Placówka może też odebrać dostęp (np. po zmianie opieki prawnej).
  - Wszystkie zmiany opiekunów są zapisywane (kto, kiedy, kto zatwierdził) jako ślad audytowy.
- **Uprawnienia i role (ustalone):**
  - **Każda operacja wymaga odpowiedniego uprawnienia.** Backend sprawdza uprawnienie przy każdej operacji (odczyt, zapis, zatwierdzanie, usuwanie itd.); interfejs tylko ukrywa niedostępne akcje.
  - **Rola = nazwany zestaw uprawnień.** Użytkownik dostaje uprawnienia wyłącznie przez role, a nie pojedynczo.
  - **Role nadawane w kontekście placówki** (lub organizacji): ta sama osoba może mieć różne role w różnych placówkach.
  - **Role podstawowe (wbudowane, domyślne):** opiekun, podopieczny, dyrektor. Każda placówka dostaje je od razu z sensownym zestawem uprawnień.
  - **Role własne:** każda placówka może tworzyć własne role i dobierać ich uprawnienia (np. nauczyciel, sekretariat, pielęgniarka, wolontariusz), a także modyfikować uprawnienia ról podstawowych w granicach dozwolonych przez system.
  - Role są definiowane **per placówka**; właściciel organizacji może nadawać role ponad placówkami. Uprawnienia systemowe, których placówka nie może nadać (np. dostęp do danych innej placówki), są zawsze poza jej kontrolą.
  - Zmiany ról i uprawnień trafiają do śladu audytowego (kto, komu, kiedy, co zmienił).
  - **Do doprecyzowania:** rola „opiekun" oznacza tu opiekuna dziecka (rodzic, opiekun prawny, dziadek itd.), czyli osobę z konta rodzica. Personel to osobne role (np. nauczyciel/wychowawca), a nie „opiekun". Nazewnictwo trzeba ustalić tak, by nie mylić tych dwóch znaczeń (np. „opiekun dziecka" vs „wychowawca").
  - **Podopieczny** to dziecko jako podmiot, któremu coś się dzieje w systemie (obecność, grupa). Zwykle nie ma własnego konta; opiekunowie działają w jego imieniu.
- Panel webowy zostaje dla zadań zarządczych (konfiguracja placówki, użytkownicy, raporty); szczegółowy podział funkcji mobilne vs web do ustalenia.
- Jeden wspólny backend dla aplikacji mobilnej i panelu webowego.
- Ułatwia to też ASO i SEO: jeden zlokalizowany wpis w sklepie, jedna marka, jeden zestaw linków do aplikacji.

## Cel: łatwa wyszukiwalność w przeglądarce, w wielu językach

Aplikacja i panel za logowaniem nie są indeksowane przez wyszukiwarki, więc potrzebna jest publiczna warstwa oraz optymalizacja w sklepach z aplikacjami.

### 1. Publiczna strona marketingowa
- Osobna od panelu, np. `earlynest.com`; panel pod `app.earlynest.com`.
- Panel ma `noindex`, żeby nie rozmywał pozycji strony.
- Dwie ścieżki treści: dla rodziców i dla placówek.
- SSG/SSR (np. Astro lub Next.js), nie SPA.
- Podstrony: funkcje, cennik, FAQ, blog, kontakt, demo dla placówek.

### 2. Wielojęzyczność
- Osobny URL dla każdego języka (`/pl/`, `/en/`, …), nie przełączanie cookie/`Accept-Language`.
- Wzajemne tagi `hreflang` + `x-default`, `<html lang>`, przetłumaczone `title` i `meta description`, własny `canonical` dla każdej wersji.
- Sitemap z wariantami językowymi, `robots.txt`.
- Strona i aplikacja mają obsługiwać dowolną liczbę języków (patrz „Wymóg: produkt globalny"). Kolejność wdrażania języków jest decyzją biznesową: proponowany start PL + EN, potem kolejne według rynków; tłumaczenia treści strony kosztują czas, więc dodawać je świadomie.
- Bez automatycznego przekierowania po IP; zamiast tego baner z propozycją zmiany języka.
- Słowa kluczowe badać osobno dla każdego języka (żłobek/przedszkole nie mają prostych odpowiedników: daycare, nursery, preschool, Kita, Kindergarten).

### 3. ASO (App Store / Google Play)
- Zlokalizować tytuł, podtytuł/krótki opis, słowa kluczowe (iOS: pole 100 znaków) i zrzuty ekranu dla każdego języka.
- Słowa kluczowe w tytule i podtytule.
- Zbierać oceny i recenzje.
- Lokalizacje ustawia się w App Store Connect i Google Play Console, niezależnie od języka telefonu.

### 4. Połączenie strony z aplikacją
- Universal Links (iOS) i App Links (Android).
- Smart App Banner na iOS.
- Linki do sklepów i JSON-LD `SoftwareApplication` (z `inLanguage`).

### 5. Wyszukiwanie lokalne
- Strona/profil dla każdej placówki oraz link lub kod QR do dołączenia (np. `earlynest.com/p/<placowka>`), otwierający aplikację z gotowym przypisaniem.
- Google Business Profile.

### 6. Treści i zaufanie
- Blog o problemie (telefony do placówki, nieobecności, komunikacja z rodzicami, RODO).
- Strona o prywatności i bezpieczeństwie danych dzieci (RODO to wymóg i sygnał zaufania).
- Opinie placówek, case study, krótkie wideo demo.

### 7. Narzędzia
Google Search Console, Bing Webmaster Tools, Lighthouse / Core Web Vitals.

## Proponowana kolejność prac (gdy przejdziemy do kodowania)

1. Strona marketingowa z PL + EN, `hreflang`, sitemapą i metadanymi.
2. Strony dla sklepów + Universal/App Links.
3. Uzupełnienie ASO.

## Otwarte decyzje

- [ ] **Model uprawnień: pierwszy element architektury** (patrz „Lista architektoniczna"). Pytania o role, uprawnienia i opiekunów poniżej rozstrzygamy w jego ramach.
- [ ] Lista architektoniczna od właściciela (patrz sekcja wyżej); od niej zależą wszystkie decyzje techniczne poniżej.
- [ ] Framework strony marketingowej: Astro (lekki, świetny pod SEO) vs Next.js (wspólny kod, jeśli panel też w React). Rozstrzygnie lista architektoniczna.
- [ ] Jakie języki na start i na jakie rynki (architektura i tak ma być na dowolną liczbę).
- [ ] Narzędzie/proces tłumaczeń (np. platforma do zarządzania tłumaczeniami, tłumaczenie ludzkie vs maszynowe).
- [ ] Czy wiadomości między placówką a rodzicem mają być automatycznie tłumaczone.
- [ ] Które rynki poza UE obsługujemy i jakie mają wymogi prawne dot. danych dzieci.
- [ ] Nazwa domeny i struktura subdomen.
- [ ] Stack aplikacji mobilnej i panelu webowego (nieustalony; priorytet: prostota, jedna kodowa baza mobilna, np. rozwiązanie cross-platform).
- [ ] Które funkcje personelu są dostępne w aplikacji mobilnej, a które tylko w panelu webowym.
- [ ] Dokładna lista uprawnień (katalog operacji) i domyślne zestawy dla ról podstawowych: opiekun, podopieczny, dyrektor (oraz właściciel organizacji i personel).
- [ ] Nazewnictwo ról: jak odróżnić „opiekuna dziecka" (rodzic, dziadek, opiekun prawny) od personelu opiekującego się dziećmi w placówce (np. wychowawca).
- [ ] Czy podopieczny (dziecko) może mieć własne konto/dostęp (np. starsze dzieci) i z jakimi uprawnieniami, czy zawsze działają za nie opiekunowie.
- [ ] Granice własnych ról placówki: które uprawnienia placówka może nadawać, a które są zastrzeżone dla systemu (np. dostęp do danych innej placówki).
- [ ] Proces dodawania opiekuna: kto go inicjuje (istniejący opiekun zaprasza, nowy opiekun prosi o dostęp przez placówkę, placówka dodaje sama), jak placówka weryfikuje tożsamość i prawo do opieki (np. dokument, kontakt osobisty, tylko decyzja personelu) oraz czy w aplikacji przechowujemy dokumenty.
- [ ] Czy istniejący opiekunowie są informowani o dodaniu nowego opiekuna i czy mogą się sprzeciwić.
- [ ] Spory o opiekę (np. rozwód): jak placówka ogranicza dostęp wybranemu opiekunowi.
- [ ] Czy dziecko może być jednocześnie w kilku placówkach (np. dwa żłobki w różne dni), czy zawsze w jednej naraz.
- [ ] Zasady retencji i dostępu do danych po zmianie placówki lub zakończeniu opieki (RODO).
- [ ] Model biznesowy / cennik.

## Lista architektoniczna (źródło decyzji technicznych)

**Kolejność (ustalone):**

1. **Najpierw modelujemy model uprawnień** (uprawnienia, role, kontekst placówki/organizacji, powiązania opiekun–dziecko, ślad audytowy). To pierwszy element architektury i podstawa dla pozostałych; kolejne elementy listy projektujemy dopiero po nim, uwzględniając jego wynik.
2. Kolejne elementy: według listy dostarczonej przez właściciela (poniżej).

**Lista od właściciela:**

- Projekt realizujemy **zgodnie z listą rzeczy architektonicznych**, którą dostarczy właściciel (jeszcze nie została dostarczona).
- Gdy lista się pojawi, zostanie dopisana w tej sekcji (lub zlinkowana do osobnego pliku, np. `docs/architecture.md`) i **ma pierwszeństwo** przy wyborze stacku, struktury i rozwiązań technicznych.
- Do tego czasu **nie podejmujemy decyzji architektonicznych ani technologicznych** (stack, baza danych, hosting, framework). Wszystko powyżej to wymagania produktowe i ograniczenia, a nie wybór technologii.
- Jeśli wymagania z tego pliku kolidują z listą architektoniczną, zgłoś konflikt właścicielowi zamiast rozstrzygać samodzielnie.

## Zasady współpracy

- Nie pisz kodu bez wyraźnej prośby.
- **Nie edytuj tego pliku samodzielnie.** Zmiany w `CLAUDE.md` wprowadzaj wyłącznie na wyraźne polecenie właściciela. Ustalenia z rozmów same nie trafiają do pliku; jeśli coś wygląda na warte zapisania, zaproponuj to w odpowiedzi i poczekaj na polecenie.
- Odpowiadaj po polsku, zwięźle.
