# Dokumentacja kodu strony internetowej

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Struktura strony](#struktura-strony)
3. [Sekcja ankiety](#sekcja-ankiety)
   - [Jak zmienić link do ankiety](#jak-zmienić-link-do-ankiety)
   - [Jak zmienić obrazki](#jak-zmienić-obrazki)
   - [Jak zmienić teksty](#jak-zmienić-teksty)
4. [Sekcja artykułów](#sekcja-artykułów)
   - [Jak dodać nowy artykuł](#jak-dodać-nowy-artykuł)
   - [Jak zmienić istniejący artykuł](#jak-zmienić-istniejący-artykuł)
5. [Style CSS](#style-css)
   - [Kolorystyka](#kolorystyka)
   - [Animacje](#animacje)
   - [Responsywność](#responsywność)
6. [Najczęstsze problemy i rozwiązania](#najczęstsze-problemy-i-rozwiązania)

## Wprowadzenie

Dokumentacja dotyczy kodu strony internetowej o tematyce oszczędzania energii. Strona składa się z dwóch głównych sekcji:
1. Sekcja ankiety - zachęcająca do wypełnienia ankiety dotyczącej zużycia energii
2. Sekcja artykułów - prezentująca artykuły związane z oszczędzaniem energii

## Struktura strony

Strona zbudowana jest w oparciu o HTML5 i CSS3. Składa się z kontenera głównego, który zawiera dwie sekcje:

```html
<div class="container">
  <!-- Sekcja ankiety -->
  <section class="survey-section">
    <!-- Zawartość sekcji ankiety -->
  </section>

  <!-- Sekcja artykułów -->
  <section class="articles-section">
    <!-- Zawartość sekcji artykułów -->
  </section>
</div>
```

## Sekcja ankiety

Sekcja ankiety ma strukturę:

```html
<section class="survey-section">
  <div class="survey-content">
    <div class="left-image">
      <img src="https://www.ur.edu.pl/files/user_directory/1274/image1.png" alt="Energy saving image">
    </div>
    <div class="survey-center">
      <h2>BADANIE ZUŻYCIA ENERGII</h2>
      <p>Wypełnij ankiete dotycząca zużycia energii standby, aby dowiedzieć się ile możesz zaoszczędzić!</p>
      <button class="fill-survey-btn"><a href="#" class="survey-link">Ankieta</a></button>
    </div>
    <div class="right-image">
      <img src="https://www.ur.edu.pl/files/user_directory/1274/image2.png" alt="Electronic devices image">
    </div>
  </div>
</section>
```

### Jak zmienić link do ankiety

Aby zmienić link do ankiety, należy edytować atrybut `href` w tagu `<a>` wewnątrz przycisku:

```html
<button class="fill-survey-btn"><a href="TUTAJ_WSTAW_NOWY_LINK" class="survey-link">Ankieta</a></button>
```

### Jak zmienić obrazki

Aby zmienić obrazki w sekcji ankiety, należy zmienić atrybut `src` w odpowiednich tagach `<img>`:

Lewy obrazek:
```html
<div class="left-image">
  <img src="TUTAJ_WSTAW_LINK_DO_NOWEGO_OBRAZKA" alt="Energy saving image">
</div>
```

Prawy obrazek:
```html
<div class="right-image">
  <img src="TUTAJ_WSTAW_LINK_DO_NOWEGO_OBRAZKA" alt="Electronic devices image">
</div>
```

### Jak zmienić teksty

Aby zmienić nagłówek sekcji ankiety:
```html
<h2>TUTAJ_WSTAW_NOWY_NAGŁÓWEK</h2>
```

Aby zmienić opis ankiety:
```html
<p>TUTAJ_WSTAW_NOWY_OPIS</p>
```

Aby zmienić tekst na przycisku:
```html
<button class="fill-survey-btn"><a href="#" class="survey-link">TUTAJ_WSTAW_NOWY_TEKST</a></button>
```

## Sekcja artykułów

Sekcja artykułów ma strukturę:

```html
<section class="articles-section">
  <h2 class="section-title">ARTYKUŁY</h2>
  <div class="articles-container">
    <!-- Tutaj znajdują się karty artykułów -->
    <article class="article-card card-1">
      <!-- Zawartość karty artykułu -->
    </article>
    <!-- Można dodać więcej kart artykułów -->
  </div>
</section>
```

### Jak dodać nowy artykuł

Aby dodać nowy artykuł, należy skopiować poniższy kod i wkleić go wewnątrz kontenera `<div class="articles-container">` (po ostatnim artykule):

```html
<article class="article-card card-2">
  <div class="article-image centered-image">
    <img src="LINK_DO_OBRAZKA_ARTYKUŁU" alt="Article thumbnail">
  </div>
  <div class="article-content">
    <h3>TYTUŁ_ARTYKUŁU</h3>
    <p>OPIS_ARTYKUŁU</p>
    <a href="LINK_DO_PEŁNEGO_ARTYKUŁU" class="read-more">
      <span>Czytaj więcej</span>
      <i class="arrow-icon">→</i>
    </a>
  </div>
</article>
```

**WAŻNE:** Pamiętaj, aby zmienić klasę `card-2` na kolejny numer dla każdego nowego artykułu (np. `card-3`, `card-4` itd.). Jest to potrzebne do animacji.

### Jak zmienić istniejący artykuł

Aby zmienić obrazek artykułu:
```html
<div class="article-image centered-image">
  <img src="TUTAJ_WSTAW_NOWY_LINK_DO_OBRAZKA" alt="Article thumbnail">
</div>
```

Aby zmienić tytuł artykułu:
```html
<h3>TUTAJ_WSTAW_NOWY_TYTUŁ</h3>
```

Aby zmienić opis artykułu:
```html
<p>TUTAJ_WSTAW_NOWY_OPIS</p>
```

Aby zmienić link "Czytaj więcej":
```html
<a href="TUTAJ_WSTAW_NOWY_LINK" class="read-more">
```

## Style CSS

### Kolorystyka

Główne kolory strony można zmodyfikować poprzez edycję następujących miejsc w kodzie CSS:

1. Kolor tła sekcji ankiety (obecnie niebieski):
```css
.survey-section {
  background-color: #0039a6; /* Tutaj zmień kolor */
}
```

2. Kolor nagłówków (obecnie niebieski):
```css
.section-title, .article-content h3 {
  color: #0039a6; /* Tutaj zmień kolor */
}
```

3. Kolor przycisków i linków:
```css
.fill-survey-btn {
  background-color: white; /* Kolor tła przycisku */
}

.survey-link {
  color: #0039a6; /* Kolor tekstu linku */
}

.read-more {
  color: #0039a6; /* Kolor tekstu "Czytaj więcej" */
  border: 2px solid #0039a6; /* Kolor obramowania */
}

.read-more:before {
  background-color: #0039a6; /* Kolor tła przycisku po najechaniu */
}
```

### Animacje

Strona zawiera kilka animacji CSS:

1. Efekt hover dla sekcji ankiety - powiększenie cienia
2. Efekt hover dla obrazków - delikatne powiększenie
3. Efekt hover dla kart artykułów - przesunięcie i obrót
4. Efekt hover dla przycisku "Czytaj więcej" - wypełnienie kolorem

Czasy trwania animacji można dostosować zmieniając wartości `transition` w CSS.

### Responsywność

Strona jest responsywna dzięki media queries:

```css
@media (max-width: 768px) {
  /* Style dla urządzeń mobilnych */
}
```

Można dodać dodatkowe breakpointy lub zmodyfikować istniejące style dla różnych rozmiarów ekranu.

## Najczęstsze problemy i rozwiązania

### Problem: Obrazki nie wyświetlają się
**Rozwiązanie:** Sprawdź czy linki do obrazków są poprawne i dostępne. Upewnij się, że serwer, na którym hostowane są obrazki, pozwala na wyświetlanie ich na zewnętrznych stronach.

### Problem: Niewłaściwe wyświetlanie na urządzeniach mobilnych
**Rozwiązanie:** Dostosuj style w sekcji media queries (`@media (max-width: 768px)`) - możesz zmniejszyć rozmiary czcionek, marginesów lub zmienić układ elementów.

### Problem: Znaki [nbsp] pojawiają się w tekście
**Rozwiązanie:** Usuń wszystkie wystąpienia `[nbsp]` z kodu HTML. To są znaczniki dla spacji nierozdzielających, które powinny być zamienione na faktyczne spacje nierozdzielające `&nbsp;` lub zwykłe spacje.

### Problem: Animacje działają nieprawidłowo
**Rozwiązanie:** Upewnij się, że dla każdego nowego artykułu używasz unikalnych klas numerowanych (np. `card-1`, `card-2` itd.) oraz że @keyframes są poprawnie zdefiniowane w CSS.


# Dokumentacja skryptu Google Apps Script do analizy zużycia energii

## Spis treści
1. [Wprowadzenie](#wprowadzenie)
2. [Struktura skryptu](#struktura-skryptu)
3. [Funkcja onFormSubmit](#funkcja-onformsubmit)
   - [Pobieranie danych z formularza](#pobieranie-danych-z-formularza)
   - [Określanie dostawcy energii](#określanie-dostawcy-energii)
   - [Obliczanie zużycia energii](#obliczanie-zużycia-energii)
   - [Wysyłanie e-maila z wynikami](#wysyłanie-e-maila-z-wynikami)
   - [Ustawianie przypomnienia](#ustawianie-przypomnienia)
4. [Funkcja parseTime](#funkcja-parsetime)
5. [Funkcja checkReminders](#funkcja-checkreminders)
6. [Arkusze danych](#arkusze-danych)
7. [Modyfikacja indeksów wartości](#modyfikacja-indeksów-wartości)
8. [Dodawanie nowych urządzeń](#dodawanie-nowych-urządzeń)
9. [Zmiany w logice obliczania kosztów](#zmiany-w-logice-obliczania-kosztów)
10. [Najczęstsze problemy i rozwiązania](#najczęstsze-problemy-i-rozwiązania)

## Wprowadzenie

Skrypt służy do analizy zużycia energii na podstawie odpowiedzi z formularza Google. Kalkuluje roczne zużycie energii przez urządzenia w trybie standby oraz związane z tym koszty. Po analizie wysyła e-mail do respondenta z wynikami oraz ustawia przypomnienie o ponownym wypełnieniu ankiety po określonym czasie.

## Struktura skryptu

Skrypt składa się z trzech głównych funkcji:
1. `onFormSubmit(e)` - główna funkcja wywoływana po przesłaniu formularza
2. `parseTime(timeStr)` - funkcja pomocnicza do przetwarzania czasów
3. `checkReminders()` - funkcja do wysyłania przypomnień o ponownym wypełnieniu ankiety

## Funkcja onFormSubmit

Funkcja `onFormSubmit(e)` jest wywoływana automatycznie po przesłaniu formularza. Przyjmuje argument `e`, który zawiera odpowiedzi z formularza.

### Pobieranie danych z formularza

Obecnie skrypt pobiera dane z formularza używając indeksów tablicy `e.values`:

```javascript
const email = e.values[32]; // Adres e-mail
let providerInput = e.values[30] ? e.values[30].trim() : ""; // Dostawca prądu
const wojewodztwo = e.values[29] ? e.values[29].trim() : "";
```

### Określanie dostawcy energii

Skrypt określa dostawcę energii na podstawie odpowiedzi użytkownika lub, jeśli nie jest podany, na podstawie województwa:

```javascript
let provider = "";
if (providerInput && providerInput !== "Nie wiem" && prices.some(row => row[0].trim() === providerInput)) {
  provider = providerInput;
} else {
  for (let i = 1; i < providers.length; i++) {
    if (providers[i][0].trim() === wojewodztwo) {
      provider = providers[i][1].trim();
      break;
    }
  }
}
```

### Obliczanie zużycia energii

Skrypt analizuje odpowiedzi dotyczące ilości urządzeń i czasu ich użytkowania, a następnie oblicza zużycie energii w trybie standby:

1. Najpierw identyfikuje indeksy odpowiedzi związanych z urządzeniami:
   ```javascript
   for (let j = 0; j < responsesHeaders.length; j++) {
     let header = responsesHeaders[j].trim();
     if (header.startsWith("Ile urządzeń masz w domu?")) {
       let deviceName = header.replace("Ile urządzeń masz w domu?", "").trim();
       deviceName = deviceName.replace(/\[|\]/g, "");
       deviceUsageIndex[deviceName] = { countIndex: j };
     } else if (deviceUsageIndex[header] !== undefined) {
       deviceUsageIndex[header].timeIndex = j;
     }
   }
   ```

2. Następnie oblicza zużycie energii dla każdego urządzenia:
   ```javascript
   for (let i = 1; i < energyData.length; i++) {
     const deviceName = energyData[i][0].trim();
     const powerUsage = parseFloat(energyData[i][1]);

     const deviceInfo = deviceUsageIndex[deviceName] || {};
     const countIndex = deviceInfo.countIndex !== undefined ? deviceInfo.countIndex : -1;
     const timeIndex = deviceInfo.timeIndex !== undefined ? deviceInfo.timeIndex : -1;

     let dailyUsage = timeIndex !== -1 ? parseTime(e.values[timeIndex]) : NaN;
     let deviceCount = countIndex !== -1 ? parseInt(e.values[countIndex]) : NaN;

     if (isNaN(dailyUsage)) dailyUsage = 0;
     if (isNaN(deviceCount)) deviceCount = 1;

     const standbyHours = 24 - dailyUsage;
     const dailyEnergyUsage = ((powerUsage * standbyHours) / 1000) * deviceCount;
     const annualDeviceEnergy = dailyEnergyUsage * 365;
     totalStandbyEnergy += annualDeviceEnergy;
   }
   ```

3. Oblicza roczny koszt energii:
   ```javascript
   const annualStandbyCost = totalStandbyEnergy * pricePerKwh;
   ```

### Wysyłanie e-maila z wynikami

Skrypt wysyła e-mail z wynikami analizy:

```javascript
GmailApp.sendEmail(email, "Ankieta - Roczne użycie energii", `
Szanowny Użytkowniku,

Dziękujemy za wypełnienie ankiety dotyczącej rocznego zużycia energii.

Oto szczegóły Twojej analizy:
- Województwo: ${wojewodztwo}
- Dostawca energii: ${provider}
- Cena za 1 kWh: ${pricePerKwh.toFixed(2)} zł
- Twoje urządzenia w trybie czuwania zużywają rocznie: ${totalStandbyEnergy.toFixed(2)} kWh
- Roczne koszty energii w trybie czuwania: ${annualStandbyCost.toFixed(2)} zł

Za 3 miesiące otrzymasz e-mail'a z prośbą o powtórne wypełnienie ankiety. Zachęcamy do ponownego wypełnienia formularza, zostanie to wykorzystane do przygotowania statystyk.

Dziękujemy za wypełnienie ankiety.
`);
```

### Ustawianie przypomnienia

Skrypt dodaje informację o przypomnieniu do arkusza "Przypomnienia":

```javascript
const reminderSheet = sheet.getSheetByName("Przypomnienia") || sheet.insertSheet("Przypomnienia");
const now = new Date();
const reminderDate = new Date(now.getTime() + 60 * 60 * 1000 * 24 * 30); // 30 dni później

reminderSheet.appendRow([
  email,
  reminderDate.toISOString(),
  "NIE" // status: wysłano przypomnienie?
]);
```

## Funkcja parseTime

Funkcja `parseTime(timeStr)` przetwarza czas podany w formacie "HH:MM:SS" na liczbę godzin:

```javascript
function parseTime(timeStr) {
  if (!timeStr) return 0;
  const parts = timeStr.split(":").map(Number);
  return parts[0] + (parts[1] / 60) + (parts[2] / 3600);
}
```

## Funkcja checkReminders

Funkcja `checkReminders()` sprawdza, czy należy wysłać przypomnienia do użytkowników:

```javascript
function checkReminders() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet();
  const reminderSheet = sheet.getSheetByName("Przypomnienia");
  if (!reminderSheet) {
    Logger.log("Brak arkusza 'Przypomnienia'");
    return;
  }

  const data = reminderSheet.getDataRange().getValues();
  const now = new Date();

  for (let i = 1; i < data.length; i++) {
    const email = data[i][0];
    const reminderRaw = data[i][1];
    const sent = data[i][2];
    
    const reminderTime = new Date(reminderRaw);

    if (sent !== "TAK" && now >= reminderTime) {
      try {
        GmailApp.sendEmail(email, "Przypomnienie - ankieta energetyczna", 
          `Minął czas od ostatniego wypełnienia ankiety. Zachęcamy do ponownego wypełnienia formularza!`);
        reminderSheet.getRange(i + 1, 3).setValue("TAK");
        Logger.log(`Wysłano przypomnienie do: ${email}`);
      } catch (error) {
        Logger.log(`Błąd przy wysyłaniu przypomnienia do ${email}: ${error.message}`);
      }
    }
  }
}
```

## Arkusze danych

Skrypt korzysta z następujących arkuszy w skoroszycie:

1. **Odpowiedzi** - zawiera odpowiedzi z formularza
2. **Ceny energii** - zawiera ceny energii dla różnych dostawców
3. **Miasta - Dostawca** - mapuje województwa na domyślnych dostawców energii
4. **Zużycie energii** - zawiera informacje o zużyciu energii przez różne urządzenia
5. **Przypomnienia** - zawiera informacje o przypomnieniach do wysłania

## Modyfikacja indeksów wartości

Jeśli struktura formularza zmieni się, konieczne będzie zaktualizowanie indeksów używanych do pobierania wartości z tablicy `e.values`:

```javascript
const email = e.values[32]; // Adres e-mail
let providerInput = e.values[30] ? e.values[30].trim() : ""; // Dostawca prądu
const wojewodztwo = e.values[29] ? e.values[29].trim() : "";
```

Aby znaleźć właściwe indeksy:
1. Dodaj kod logujący na początku funkcji `onFormSubmit`:
   ```javascript
   Logger.log("Dane z formularza:");
   for (let i = 0; i < e.values.length; i++) {
     Logger.log(`[${i}]: ${e.values[i]}`);
   }
   ```
2. Prześlij testowy formularz
3. Sprawdź logi, aby znaleźć właściwe indeksy

## Dodawanie nowych urządzeń

Aby dodać nowe urządzenie do analizy:

1. Dodaj pytanie do formularza Google w formacie "Ile urządzeń masz w domu? [Nazwa urządzenia]"
2. Dodaj pytanie o czas użytkowania urządzenia (nazwa pytania powinna być identyczna z nazwą urządzenia)
3. Dodaj urządzenie do arkusza "Zużycie energii" z jego mocą w watach w trybie standby

## Zmiany w logice obliczania kosztów

Jeśli chcesz zmienić logikę obliczania kosztów, możesz zmodyfikować następujący fragment kodu:

```javascript
const standbyHours = 24 - dailyUsage;
const dailyEnergyUsage = ((powerUsage * standbyHours) / 1000) * deviceCount;
const annualDeviceEnergy = dailyEnergyUsage * 365;
```

Na przykład, aby uwzględnić różne stawki dzienne i nocne, możesz zmodyfikować ten kod, dodając odpowiednie mnożniki dla różnych godzin.

## Najczęstsze problemy i rozwiązania

### Problem: Zmiana struktury formularza powoduje błędy

**Rozwiązanie:** Zaktualizuj indeksy w tablicy `e.values`. Dodaj tymczasowy kod logujący, aby znaleźć właściwe indeksy.

### Problem: Brak dopasowania nazw urządzeń

**Rozwiązanie:** Upewnij się, że nazwy urządzeń w arkuszu "Zużycie energii" dokładnie odpowiadają nazwom używanym w pytaniach formularza (po usunięciu "Ile urządzeń masz w domu?" i nawiasów kwadratowych).

### Problem: Nieprawidłowe obliczenia zużycia energii

**Rozwiązanie:** Sprawdź, czy wartości mocy w arkuszu "Zużycie energii" są poprawne i wyrażone w watach. Sprawdź też, czy funkcja `parseTime` poprawnie przetwarza czas z formatu używanego w odpowiedziach.

### Problem: E-maile nie są wysyłane

**Rozwiązanie:** Upewnij się, że masz odpowiednie uprawnienia do wysyłania e-maili przez GmailApp. Sprawdź też, czy adres e-mail jest poprawny i czy nie przekraczasz dziennego limitu wysyłanych e-maili.

### Problem: Przypomnienia nie są wysyłane

**Rozwiązanie:** Upewnij się, że funkcja `checkReminders` jest uruchamiana regularnie za pomocą wyzwalacza czasowego. Sprawdź też, czy arkusz "Przypomnienia" ma poprawną strukturę (e-mail w pierwszej kolumnie, data przypomnienia w drugiej, status w trzeciej).
