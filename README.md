### **Lekcja: Konfiguracja Karty Sieciowej w Ubuntu za pomocą Terminala i Midnight Commander**

---

#### **Cel Lekcji**

- Nauczyć się, jak skonfigurować kartę sieciową w Ubuntu.
- Poznać, jak korzystać z programu Midnight Commander (MC) do zarządzania i edycji plików.
- Nauczyć się kopiować zawartość przykładowego pliku konfiguracyjnego do istniejącego pliku konfiguracyjnego sieci.

---

#### **Wymagania Wstępne**

- Podstawowa znajomość obsługi komputera.
- Zainstalowany system Ubuntu (wersja terminalowa).
- Dostęp do konta z uprawnieniami administratora (sudo).
- Zainstalowany program Midnight Commander (MC).

**Instalacja MC (jeśli nie jest zainstalowany):**

```bash
sudo apt update
sudo apt install mc
```

---

### **Materiały Potrzebne**

- Komputer z zainstalowanym Ubuntu.
- Dostęp do terminala.
- Notatnik do zapisywania ważnych informacji.

---

### **Kroki Konfiguracji Karty Sieciowej**

---

#### **1. Uruchomienie Midnight Commander (MC)**

- **Cel:** Otworzyć program MC z uprawnieniami administratora.

- **Krok:**

  W terminalu wpisz:

  ```bash
  sudo mc
  ```

  **Wyjaśnienie:**

  - `sudo` pozwala uruchomić program z uprawnieniami administratora.
  - `mc` to skrót od Midnight Commander, programu do zarządzania plikami.

---

#### **2. Nawigacja do Katalogu Konfiguracyjnego Sieci**

- **Cel:** Znaleźć istniejący plik konfiguracyjny sieci.

- **Kroki:**

  - W lewym panelu MC:

    - Użyj strzałek, aby przejść do katalogu `etc`.

    - Naciśnij `Enter`, aby wejść do katalogu `etc`.

    - Znajdź i wejdź do katalogu `netplan`.

    - Teraz powinieneś być w katalogu `/etc/netplan/`.

  - W tym katalogu powinien znajdować się plik konfiguracyjny, np. `00-installer-config.yaml`.

---

#### **3. Otworzenie Istniejącego Pliku Konfiguracyjnego do Edycji**

- **Cel:** Otworzyć plik konfiguracyjny sieci w edytorze MC.

- **Kroki:**

  - W lewym panelu MC:

    - Zaznacz istniejący plik konfiguracyjny (np. `00-installer-config.yaml`).

    - Naciśnij klawisz `F4` (Edytuj), aby otworzyć plik w edytorze MC.

  - Plik może wyglądać następująco:

    ```yaml
    network:
      ethernets:
        enp0s3:
          dhcp4: true
      version: 2
    ```

  **Wyjaśnienie:**

  - Plik ten zawiera aktualną konfigurację sieci, którą będziemy modyfikować.

---

#### **4. Otworzenie Przykładowego Pliku Konfiguracyjnego**

- **Cel:** Skopiować zawartość przykładowego pliku konfiguracyjnego.

- **Kroki:**

  - W prawym panelu MC:

    - Użyj strzałek, aby przejść do katalogu z przykładami plików.

      - Naciśnij klawisz `/` (ukośnik), wpisz `usr` i naciśnij `Enter`, aby przejść do katalogu głównego, a następnie do `usr`.

      - Przejdź do katalogu `share`.

      - Następnie do `doc`.

      - Potem do `netplan`.

      - I wreszcie do `examples`.

      - Teraz powinieneś być w katalogu `/usr/share/doc/netplan/examples`.

    - Znajdź plik `01-netcfg.yaml`.

    - Zaznacz plik `01-netcfg.yaml`.

    - Naciśnij klawisz `F4` (Edytuj), aby otworzyć go w edytorze MC.

  **Wyjaśnienie:**

  - Otwieramy przykładowy plik konfiguracyjny, aby skopiować z niego zawartość.

---

#### **5. Kopiowanie Zawartości Przykładowego Pliku do Istniejącego Pliku**

- **Cel:** Skopiować konfigurację z przykładowego pliku do naszego pliku konfiguracyjnego.

- **Kroki:**

  - **W edytorze MC z otwartym plikiem `01-netcfg.yaml` (prawy panel):**

    - Naciśnij `F9`, aby otworzyć menu na górze.

    - Przejdź do menu `Edit`.

    - Wybierz opcję `Select All` lub naciśnij skrót `Shift+F5`.

    - Wszystka zawartość powinna zostać zaznaczona.

    - Naciśnij `F9` ponownie, przejdź do `Edit` i wybierz `Copy` lub naciśnij `Alt+6` (w niektórych wersjach może to być `Ctrl+Insert`).

  - **Przełącz się do edytora z otwartym plikiem istniejącej konfiguracji (lewy panel):**

    - Jeśli edytor nie jest widoczny, możesz przełączać się między otwartymi edytorami za pomocą `Alt+Tab` w MC.

    - Umieść kursor w miejscu, gdzie chcesz wkleić zawartość (najlepiej zastąpić całą istniejącą konfigurację).

    - Naciśnij `F9`, przejdź do `Edit` i wybierz `Paste` lub naciśnij `Shift+Insert`.

    - Zawartość przykładowego pliku powinna zostać wklejona.

  **Wyjaśnienie:**

  - Kopiujemy zawartość przykładowego pliku do naszego pliku konfiguracyjnego, aby mieć gotowy szablon do edycji.

---

#### **6. Dostosowanie Pliku Konfiguracyjnego do Własnych Potrzeb**

- **Cel:** Edytować konfigurację, aby pasowała do naszej sieci.

- **Kroki:**

  - **W edytorze MC z otwartym plikiem istniejącej konfiguracji:**

    - Upewnij się, że plik zawiera następującą strukturę:

      ```yaml
      network:
        version: 2
        renderer: networkd
        ethernets:
          enp3s0:
            dhcp4: no
            addresses:
              - 192.168.0.2/24
            gateway4: 192.168.0.1
            nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
      ```

    - **Zmiany do wprowadzenia:**

      - **Nazwa interfejsu sieciowego (`enp3s0`):**

        - Sprawdź nazwę swojego interfejsu sieciowego.

          - W MC naciśnij `Ctrl+O`, aby przejść do terminala.

          - Wpisz:

            ```bash
            ip link show
            ```

          - Znajdź nazwę interfejsu, który jest aktywny (np. `enp0s3`).

          - Naciśnij `Ctrl+O`, aby wrócić do MC.

        - Zmień `enp3s0` na właściwą nazwę interfejsu (np. `enp0s3`).

      - **Adres IP (`192.168.0.2/24`):**

        - Ustaw żądany statyczny adres IP (np. `192.168.1.100/24`).

      - **Brama domyślna (`192.168.0.1`):**

        - Ustaw adres swojej bramy sieciowej (np. `192.168.1.1`).

      - **Serwery DNS (`8.8.8.8, 8.8.4.4`):**

        - Możesz zostawić domyślne lub wpisać inne, np. `1.1.1.1, 1.0.0.1`.

    - **Przykład zmodyfikowanego pliku:**

      ```yaml
      network:
        version: 2
        renderer: networkd
        ethernets:
          enp0s3:
            dhcp4: no
            addresses:
              - 192.168.1.100/24
            gateway4: 192.168.1.1
            nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
      ```

    - **Zapisanie zmian:**

      - Naciśnij klawisz `F2` (Zapisz).

      - Upewnij się, że plik został zapisany bez błędów.

  **Wyjaśnienie:**

  - Dostosowujemy konfigurację sieci do naszych potrzeb, ustawiając statyczny adres IP, bramę i serwery DNS.

---

#### **7. Zastosowanie Nowej Konfiguracji**

- **Cel:** Wprowadzić zmiany w konfiguracji sieci.

- **Kroki:**

  - Zamknij edytor MC (jeśli jest otwarty), naciskając `F10`.

  - W terminalu wpisz:

    ```bash
    sudo netplan apply
    ```

  - Jeśli pojawią się błędy, program wyświetli komunikat.

    - W takim przypadku wróć do edycji pliku i sprawdź, czy nie ma literówek lub błędów w formatowaniu.

  **Wyjaśnienie:**

  - `netplan apply` wprowadza zmiany w konfiguracji sieci na podstawie plików w katalogu `/etc/netplan/`.

---

#### **8. Weryfikacja Konfiguracji Sieciowej**

- **Cel:** Sprawdzić, czy sieć działa poprawnie.

- **Kroki:**

  - W terminalu wpisz:

    ```bash
    ip addr show
    ```

    - Sprawdź, czy interfejs sieciowy ma ustawiony nowy adres IP.

  - Sprawdź połączenie z internetem, wpisując:

    ```bash
    ping -c 4 8.8.8.8
    ```

    - Jeśli otrzymasz odpowiedzi (czas i statystyki), oznacza to, że połączenie działa.

  **Wyjaśnienie:**

  - `ip addr show` wyświetla informacje o interfejsach sieciowych i ich adresach IP.

  - `ping -c 4 8.8.8.8` wysyła 4 pakiety do serwera DNS Google, aby sprawdzić połączenie.

---

### **Ćwiczenia Praktyczne**

---

1. **Sprawdzenie Nazwy Interfejsu Sieciowego**

   - W terminalu wpisz:

     ```bash
     ip link show
     ```

   - Zapisz nazwę interfejsu (np. `enp0s3`).

2. **Instalacja Midnight Commander (jeśli nie jest zainstalowany)**

   - Wpisz w terminalu:

     ```bash
     sudo apt update
     sudo apt install mc
     ```

3. **Kopiowanie Zawartości Przykładowego Pliku Konfiguracyjnego**

   - Uruchom MC z uprawnieniami administratora:

     ```bash
     sudo mc
     ```

   - Otwórz istniejący plik konfiguracyjny w katalogu `/etc/netplan/`.

   - Otwórz przykładowy plik konfiguracyjny w katalogu `/usr/share/doc/netplan/examples/`.

   - Skopiuj zawartość przykładowego pliku do istniejącego pliku konfiguracyjnego.

4. **Modyfikacja Pliku Konfiguracyjnego**

   - Dostosuj ustawienia w pliku konfiguracyjnym do swojej sieci (nazwa interfejsu, adres IP, brama, DNS).

5. **Zastosowanie i Weryfikacja Nowej Konfiguracji**

   - Zastosuj konfigurację:

     ```bash
     sudo netplan apply
     ```

   - Sprawdź adres IP:

     ```bash
     ip addr show
     ```

   - Przetestuj połączenie:

     ```bash
     ping -c 4 8.8.8.8
     ```

---

### **Podsumowanie**

W tej lekcji nauczyłeś się:

- Jak korzystać z programu Midnight Commander do zarządzania i edycji plików systemowych.

- Jak skopiować zawartość przykładowego pliku konfiguracyjnego do istniejącego pliku.

- Jak dostosować konfigurację sieci do własnych potrzeb.

- Jak zastosować nową konfigurację i sprawdzić jej poprawność.

---

### **Słowniczek Terminów**

- **Terminal** - narzędzie do wprowadzania poleceń tekstowych w systemie operacyjnym.

- **sudo** - polecenie pozwalające na wykonywanie operacji z uprawnieniami administratora.

- **Midnight Commander (MC)** - menedżer plików działający w terminalu.

- **Interfejs sieciowy** - urządzenie lub program umożliwiający komunikację sieciową.

- **Adres IP** - unikalny adres identyfikujący urządzenie w sieci.

- **Brama domyślna** - urządzenie (np. router) umożliwiające komunikację z innymi sieciami.

- **Serwer DNS** - serwer tłumaczący nazwy domen (np. www.google.com) na adresy IP.

---

### **Dodatkowe Wskazówki**

- **Uważaj na wcięcia w pliku YAML**: Pliki konfiguracyjne w formacie YAML są czułe na wcięcia. Używaj spacji (najlepiej dwóch) do wcięć, nie używaj tabulatorów.

- **Sprawdzaj dokładnie pisownię**: Literówki w nazwach mogą powodować błędy.

- **Jeśli masz problem**: Sprawdź komunikaty błędów po `netplan apply` i popraw plik konfiguracyjny.

---

### **Dodatkowe Zasoby**

- [Dokumentacja Netplan](https://netplan.io/)

- [Instrukcja Midnight Commander](https://midnight-commander.org/wiki/doc)

---

Jeśli masz pytania lub napotkasz problemy, poproś nauczyciela o pomoc.

---
