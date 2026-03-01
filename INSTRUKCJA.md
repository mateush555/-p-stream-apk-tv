# 📺 PStream TV — Aplikacja na Android TV

Aplikacja Android TV łącząca się ze stroną pstream.mov.
Posiada: przeglądanie filmów, wyszukiwanie, odtwarzanie wideo.

---

## 🔧 Co potrzebujesz (instalacja raz)

### 1. Zainstaluj Javę (JDK)
- Wejdź na: https://adoptium.net/
- Pobierz **JDK 17** dla Windows
- Zainstaluj normalnie (klikaj Dalej)

### 2. Zainstaluj Android Studio
- Wejdź na: https://developer.android.com/studio
- Pobierz i zainstaluj Android Studio
- Przy pierwszym uruchomieniu zaakceptuj wszystko i kliknij "Finish"

---

## 📂 Otwieranie projektu

1. Otwórz **Android Studio**
2. Kliknij **"Open"** (lub File → Open)
3. Wybierz folder **`PStreamTV`** (ten który właśnie pobrałeś)
4. Poczekaj aż Android Studio pobierze wszystkie biblioteki (może trwać 5-10 minut przy pierwszym otwarciu)
5. Na dole zobaczysz pasek postępu — poczekaj aż zniknie

---

## 🏗️ Budowanie APK

### Opcja A — przez menu (łatwiejsza)
1. W górnym menu kliknij **Build**
2. Kliknij **"Build Bundle(s) / APK(s)"**
3. Kliknij **"Build APK(s)"**
4. Poczekaj na budowanie (1-3 minuty)
5. Po zakończeniu zobaczysz powiadomienie "APK generated" — kliknij **"locate"**
6. Plik `.apk` znajdziesz w: `app/build/outputs/apk/debug/app-debug.apk`

### Opcja B — przez terminal
```
gradlew assembleDebug
```

---

## 📱 Instalacja na Android TV

### Metoda 1 — USB (najprostsza)
1. Podłącz Android TV do komputera kablem USB
2. Na TV wejdź w **Ustawienia → Preferencje urządzenia → Informacje**
3. Kliknij 7 razy "Numer kompilacji" (odblokujesz tryb developera)
4. Wróć do Ustawień → **Opcje programisty → Debugowanie USB = WŁ**
5. W Android Studio kliknij zielony przycisk ▶️ Run
6. Wybierz swoje TV z listy urządzeń

### Metoda 2 — ADB przez Wi-Fi
1. TV i komputer muszą być w tej samej sieci Wi-Fi
2. Na TV: Ustawienia → Opcje programisty → Debugowanie przez sieć = WŁ
3. Zanotuj adres IP TV (widoczny w ustawieniach)
4. Otwórz terminal i wpisz:
   ```
   adb connect [IP_TWOJEGO_TV]:5555
   adb install app-debug.apk
   ```

### Metoda 3 — Pendrive
1. Skopiuj plik `app-debug.apk` na pendrive
2. Podłącz pendrive do TV
3. Użyj menedżera plików na TV (np. "ES File Explorer") i zainstaluj apk

---

## 🎮 Jak używać aplikacji

| Przycisk pilota | Akcja |
|---|---|
| ↑ ↓ ← → | Nawigacja po filmach |
| OK / Enter | Wybierz / Play/Pause |
| ← (lewy) | Cofnij o 10 sekund |
| → (prawy) | Przewiń o 10 sekund |
| 🔍 (szukaj) | Otwórz wyszukiwarkę |
| Back | Wróć / Wyjdź |

---

## ⚠️ Ważne informacje

### Możliwe problemy
- **"Nie znaleziono źródeł wideo"** — strona pstream.mov może wymagać logowania lub blokować scraping. W takim przypadku aplikacja wykryje iframe z odtwarzaczem.
- **"403 Forbidden"** — strona blokuje boty. Możliwe że selektory HTML wymagają dostosowania.
- **Czarny ekran podczas odtwarzania** — strona może używać DRM lub skomplikowanego JavaScript do generowania linków.

### Dostosowanie selektorów HTML
Jeśli strona zmieni strukturę HTML, edytuj plik:
`app/src/main/java/com/pstream/tv/data/api/PStreamScraper.kt`

Zmień selektory CSS w metodach `parseMovieCards()` i `getVideoSources()`.

---

## 📁 Struktura projektu

```
PStreamTV/
├── app/src/main/java/com/pstream/tv/
│   ├── ui/
│   │   ├── MainActivity.kt          ← Główna aktywność
│   │   ├── MainViewModel.kt         ← Logika UI
│   │   ├── home/
│   │   │   ├── HomeFragment.kt      ← Ekran główny z listą filmów
│   │   │   └── MovieCardPresenter.kt ← Wygląd kart filmów
│   │   ├── search/
│   │   │   └── SearchActivity.kt    ← Wyszukiwarka
│   │   └── player/
│   │       └── PlayerActivity.kt    ← Odtwarzacz wideo
│   └── data/
│       ├── api/
│       │   └── PStreamScraper.kt    ← Pobiera dane ze strony
│       ├── model/
│       │   └── Models.kt            ← Modele danych
│       └── repository/
│           └── MovieRepository.kt   ← Warstwa danych
└── app/src/main/res/                ← Zasoby (layouty, grafiki)
```

---

## 🔄 Aktualizacja aplikacji

Gdy strona pstream.mov zmieni wygląd:
1. Otwórz `PStreamScraper.kt`
2. Zaktualizuj selektory CSS w metodzie `parseMovieCards()`
3. Uruchom ponownie budowanie APK

---

*Aplikacja zbudowana na: Kotlin, Android Leanback UI, ExoPlayer, Jsoup, OkHttp*
