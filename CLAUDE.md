# Earlynest – wiedza o projekcie

Ten plik jest źródłem prawdy o projekcie. Aktualizuj go, gdy coś ustalimy lub zmienimy.
Język rozmowy z właścicielem: polski.

## Czym jest Earlynest

- Aplikacja mobilna (iOS i Android) dla rodziców, których dzieci chodzą do żłobków i przedszkoli.
- Panel webowy dla placówek (żłobków/przedszkoli) do zarządzania.
- **Problem, który rozwiązujemy:** wiele placówek nie ma żadnego systemu komunikacji z rodzicami, więc rodzice muszą dzwonić.

## Odbiorcy (dwie grupy, różne potrzeby)

1. **Rodzice** – korzystają z aplikacji mobilnej. Szukają np. „aplikacja dla rodziców przedszkole", „jak zgłosić nieobecność dziecka w żłobku".
2. **Placówki** – korzystają z panelu webowego. Szukają np. „system do zarządzania przedszkolem", „program dla żłobka", „komunikacja z rodzicami".

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

- [ ] Framework strony marketingowej: Astro (lekki, świetny pod SEO) vs Next.js (wspólny kod, jeśli panel też w React).
- [ ] Jakie języki na start i na jakie rynki (architektura i tak ma być na dowolną liczbę).
- [ ] Narzędzie/proces tłumaczeń (np. platforma do zarządzania tłumaczeniami, tłumaczenie ludzkie vs maszynowe).
- [ ] Czy wiadomości między placówką a rodzicem mają być automatycznie tłumaczone.
- [ ] Które rynki poza UE obsługujemy i jakie mają wymogi prawne dot. danych dzieci.
- [ ] Nazwa domeny i struktura subdomen.
- [ ] Stack aplikacji mobilnej i panelu webowego (nieustalony).
- [ ] Model biznesowy / cennik.

## Zasady współpracy

- Nie pisz kodu bez wyraźnej prośby.
- Ustalenia z rozmów dopisuj do tego pliku (sekcje wyżej, a nierozstrzygnięte rzeczy do „Otwarte decyzje").
- Odpowiadaj po polsku, zwięźle.
