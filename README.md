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
