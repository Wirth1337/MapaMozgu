---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-07-29
last_updated: 2025-07-29
---

# [[Linux Main|Linux]]

# Protokoły pulpitu zdalnego

Protokoły pulpitu zdalnego umożliwiają administratorom zdalny dostęp graficzny do komputerów z systemami Windows, Linux czy macOS. Dzięki nim można zarządzać, rozwiązywać problemy i instalować oprogramowanie, jakby fizycznie znajdowało się przy maszynie docelowej. W systemach Windows powszechnie używany jest protokół **RDP (Remote Desktop Protocol)**, natomiast w świecie Linux często spotkamy **VNC (Virtual Network Computing)** lub system X11. macOS natomiast udostępnia własny mechanizm zdalny (np. „Screen Sharing” lub **Apple Remote Desktop**), który oparty jest na VNC. Administrator wybiera odpowiedni protokół („klucz”) do systemu, na którym pracuje – RDP dla Windows, VNC/X11 dla Linuxa, itp.

---
## RDP (Remote Desktop Protocol)

**RDP** to protokół opracowany przez Microsoft do zdalnego pulpitu Windows. Jest to bezpieczny protokół sieciowy, który umożliwia wirtualny dostęp do pulpitu zdalnej maszyny. Domyślnie RDP używa szyfrowania (np. TLS) dla ochrony przesyłanych danych[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=Remote%20Desktop%20Protocol%20is%20a,network%20managers%20remotely%20diagnose%20problems)[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=By%20default%2C%20RDP%20includes%20encryption%2C,critical%20information%20throughout%20remote%20sessions). Pozwala to administratorom na łączenie się zdalnie z serwerem lub desktopem Windows, a następnie wysyłanie komend klawiatury i myszy do maszyny zdalnej oraz odbieranie obrazu pulpitu przez sieć[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=By%20default%2C%20RDP%20includes%20encryption%2C,critical%20information%20throughout%20remote%20sessions). Typowo RDP nasłuchuje na porcie TCP **3389**, choć można to zmienić dla bezpieczeństwa.

RDP jest wspierany na wielu platformach – oprócz Windows istnieją też klienty i serwery RDP dla Linuxa, macOS, a nawet Androida czy iOS[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=and%20lets%20network%20managers%20remotely,diagnose%20problems). Na Linuxie można np. zainstalować **xrdp** – otwartoźródłowy serwer RDP, który umożliwia połączenia z dowolnych klientów RDP (np. natywny `mstsc.exe` w Windowsie)[github.com](https://github.com/neutrinolabs/xrdp#:~:text=xrdp%20provides%20a%20graphical%20login,a%20variety%20of%20RDP%20clients)[github.com](https://github.com/neutrinolabs/xrdp#:~:text=RDP%20transport%20is%20encrypted%20using,TLS%20by%20default). Domyślnie xrdp szyfruje transmisję przy pomocy TLS[github.com](https://github.com/neutrinolabs/xrdp#:~:text=RDP%20transport%20is%20encrypted%20using,TLS%20by%20default). Dzięki temu użytkownicy Windows mogą używać znanego im pulpitu zdalnego także do łączenia się z maszynami z Linuxem.

_Przykład:_ Po instalacji xrdp na Ubuntu, wystarczy włączyć usługę i zezwolić na ruch do portu 3389. Następnie można z Windows uruchomić **Połączenie pulpitu zdalnego** i podać adres IP serwera Linux.

> **Zalecenie:** RDP jest domyślnie szyfrowany i bezpieczny, ale warto używać silnych haseł oraz w razie potrzeby ograniczać dostęp np. przez VPN lub reguły zapory.

---
## VNC (Virtual Network Computing)

**VNC** to protokół graficznego pulpitu zdalnego oparty na RFB (Remote Frame Buffer). Umożliwia on zdalny dostęp do paska pulpitu i kontrolowanie myszy i klawiatury tak, jakbyśmy siedzieli przy komputerze docelowym. VNC działa na wielu systemach – jest dostępny zarówno na Linuxie, Windows jak i macOS (np. **Screen Sharing** w macOS to w istocie VNC). Istnieje wiele implementacji VNC, np. **TigerVNC**, **TightVNC**, **RealVNC**, **UltraVNC**. W Linuxie często serwery VNC uruchamia się razem z lekkimi środowiskami pulpitu (np. XFCE), szczególnie do pracy na serwerze bez lokalnego wyświetlacza.

- **Porty:** Serwer VNC standardowo nasłuchuje na TCP **5900 + N**, gdzie N to numer wyświetlacza (display). Domyślny wyświetlacz `:0` jest na porcie 5900, `:1` na 5901 itd[unix.stackexchange.com](https://unix.stackexchange.com/questions/333969/how-do-x-clients-know-that-they-will-need-to-connect-to-tcp-port-6000display-n#:~:text=,1%29%20man%20page%20for%20details).
    
- **Konfiguracja:** Wiele implementacji wymaga ustawienia hasła VNC (`vncpasswd`) oraz pliku startowego (`xstartup`) definiującego, jakie środowisko pulpitu ma się uruchomić. Przykładowo można zainstalować serwer TigerVNC i lekki desktop XFCE, a następnie utworzyć konfigurację:

```
sudo apt update
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
vncpasswd
# (Ustaw hasło dla połączeń VNC)
```

Następnie tworzymy pliki konfiguracyjne w katalogu `~/.vnc`:

```
# Utworzenie pustych plików
touch ~/.vnc/xstartup ~/.vnc/config

# Wypełnienie xstartup, np. uruchomienie XFCE4
cat <<EOT >> ~/.vnc/xstartup
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r \$HOME/.Xresources ] && xrdb \$HOME/.Xresources
x-window-manager &
EOT

# Ustawienia wyświetlania (rozdzielczość, DPI)
cat <<EOT >> ~/.vnc/config
geometry=1920x1080
dpi=96
EOT

chmod +x ~/.vnc/xstartup
```

Po takiej konfiguracji możemy uruchomić serwer VNC

`vncserver`

Po wystartowaniu zobaczymy informację o nowej sesji, np. `desktop at :1`, co oznacza, że serwer nasłuchuje na porcie **5901**. Sprawdzić aktywne sesje można poleceniem:

`vncserver -list`

**Uwaga dot. bezpieczeństwa:** W odróżnieniu od RDP, większość serwerów VNC **domyślnie nie szyfruje** przesyłanych danych. Oznacza to, że ruch pomiędzy klientem a serwerem jest w postaci otwartego tekstu (pulpit jest przesyłany bez szyfrowania)[pentestpartners.com](https://www.pentestpartners.com/security-blog/vnc-rdp-for-all-to-see/#:~:text=,also%20version%20specific%20exploits%20for). Apple zwraca uwagę, że niektóre VNC-klienty mogą nie szyfrować klawiszy i myszy, dzięki czemu poufne dane mogą zostać przechwycone[support.apple.com](https://support.apple.com/guide/remote-desktop/virtual-network-computing-access-and-control-apde0dd523e/mac#:~:text=WARNING%3A%20Allowing%20a%20non,screen%20provides%20nearly%20unrestricted%20access). Z tego powodu zaleca się:

- albo używać bezpiecznej odmiany VNC (np. RealVNC Connect czy UltraVNC w trybie szyfrowanym),
    
- albo tunelować ruch VNC przez SSH:
`ssh -L 5901:127.0.0.1:5901 -N -f -l użytkownik serwer_linux`

Następnie łączymy się lokalnie, np. klientem VNC na `localhost:5901`. Dzięki tunelowi SSH cała komunikacja będzie zaszyfrowana.

Wśród narzędzi klienckich można wymienić np. `xtightvncviewer` lub `vncviewer`:
`xtightvncviewer localhost:5901 # Podajemy hasło ustalone wcześniej.`

VNC często stosuje się do zdalnego wsparcia lub współdzielenia ekranu (np. współpraca, prezentacje). Istnieją dwa tryby serwera:

- **Serwer pełnego pulpitu** – udostępnia bieżącą sesję graficzną komputera hosta (jeśli ktoś siedzi przy klawiaturze/myszy, to będzie jednocześnie używał tej samej sesji).
    
- **Serwer wirtualnej sesji** – każdy klient loguje się do oddzielnej, wirtualnej sesji (jak serwer terminali).
    

_Najpopularniejsze implementacje VNC:_ TigerVNC, TightVNC, RealVNC, UltraVNC. Wiele z nich oferuje opcje szyfrowania (np. RealVNC szyfruje 128/256-bit AES w płatnych edycjach). Jeśli korzystamy z VNC, zawsze zaleca się sprawdzić, czy połączenie jest szyfrowane – jeśli nie, to użyć tunelowania przez SSH.

---

## X11 i serwer X (XServer)

**X Window System (X11)** to architektura graficzna powszechna w systemach Unix/Linux. Składa się z _serwera X_ (aplikacji obsługującej ekran, klawiaturę i mysz) oraz _klientów X_ (programów, które wyświetlają okna). Serwer X działa zazwyczaj na maszynie lokalnej (czyli u użytkownika, prezentując interfejs), a klient X działa na maszynie zdalnej. Dzięki koncepcji _przezroczystości sieciowej_ (network transparency), X11 pozwala na uruchamianie aplikacji na zdalnym komputerze i wyświetlanie ich okien lokalnie przez sieć. Model klient‑serwer X11 często bywa nieintuicyjny: serwer X obsługuje ekran i urządzenia, a klient X na zdalnej maszynie wysyła do niego polecenia rysowania okien i pobiera dane wejściowe.

- **Porty X11:** Standardowo X serwer nasłuchuje na porcie **6000 + N**, gdzie N to numer wyświetlacza. Dla pierwszej sesji (display `:0`) jest to port 6000, dla `:1` – port 6001 itd[unix.stackexchange.com](https://unix.stackexchange.com/questions/333969/how-do-x-clients-know-that-they-will-need-to-connect-to-tcp-port-6000display-n#:~:text=,1%29%20man%20page%20for%20details). Na przykład gdy logujemy się do Ubuntu na konsoli graficznej, X serwer zwykle nasłuchuje na `localhost:6000`. Można wyłączyć nasłuchiwanie TCP (np. dla bezpieczeństwa), używając opcji `-nolisten tcp` przy uruchamianiu Xorg.
    
- **Zdalne aplikacje X:** Gdy oba komputery to Unix/Linux z X11, nie potrzebujemy dodatkowych protokołów – można skorzystać z mechanizmu X11 forwarding przez SSH. W pliku konfiguracyjnym SSHD na serwerze musi być dozwolone `X11Forwarding yes`. Przykład sprawdzenia i włączenia:
	`cat /etc/ssh/sshd_config | grep X11Forwarding
Następnie na komputerze klienta uruchamiamy aplikację X na serwerze, np.:
`ssh -X użytkownik@zdalny_serwer /usr/bin/firefox

Powoduje to uruchomienie Firefoksa na serwerze, ale jego okno pojawi się na naszym lokalnym pulpicie. Cała komunikacja odbywa się w ramach sesji SSH – jest więc bezpiecznie zaszyfrowana.

- **Domyślne środowisko:** Serwer X jest dołączany niemal do każdej instalacji graficznej Linuksa (np. Ubuntu), jednak nowsze dystrybucje coraz częściej używają **Wayland** jako domyślnego serwera graficznego. Wayland domyślnie **nie wspiera** zdalnego renderowania i przesyłania okien przez sieć[wayland.freedesktop.org](https://wayland.freedesktop.org/faq.html#:~:text=Is%20Wayland%20network%20transparent%20%2F,does%20it%20support%20remote%20rendering). Jeśli używamy Wayland, zamiast klasycznego X11 potrzebne są inne rozwiązania (np. projekt **Waypipe**, który umożliwia przekazywanie aplikacji podobnie do `ssh -X`, czy wbudowane serwery zdalne RDP/VNC w takich środowiskach jak GNOME/KDE[wayland.freedesktop.org](https://wayland.freedesktop.org/faq.html#:~:text=As%20of%202020%2C%20there%20are,X)).
    
    - _Uwaga:_ Jeśli korzystamy z Ubuntu 22.04 LTS lub nowszego, to po zalogowaniu może być automatycznie włączony Wayland. Na loginie można wybrać sesję „Ubuntu on Xorg”, aby mieć X11. W skrócie – X11 nadal istnieje i działa, ale nie jest już jedyną opcją.
        

**Bezpieczeństwo X11:** Protokół X11 sam w sobie nie szyfruje danych – zarówno na localhost, jak i przez sieć przesyła wszystko w czystym tekście. Oznacza to, że osoba podsłuchująca ruch (np. atak MITM) może zobaczyć zawartość okien lub nawet przechwycić wprowadzone hasła[calcomsoftware.com](https://calcomsoftware.com/x-display-manager-control-protocol-xdmcp-explained/#:~:text=Leaving%20XDMCP%20enabled%20has%20several,credentials%2C%20session%20data%2C%20or%20other)[tenable.com](https://www.tenable.com/plugins/nessus/10891#:~:text=Note%20that%20XDMCP%20is%20vulnerable,keystrokes%20entered%20by%20the%20user). Ponadto na systemie docelowym można użyć narzędzi (np. `xwd`, `xgrabsc`) by bez wiedzy użytkownika robić zrzuty ekranu innych okien. To sprawia, że niebezpieczne jest uruchamianie X11 bezpośrednio przez niezabezpieczone połączenie TCP. Zaleca się zawsze tunelować X11 przez SSH albo używać alternatywnych protokołów (jak RDP przez xrdp lub VNC przez zaszyfrowane łącze).

Historyczne luki bezpieczeństwa w Xorg potwierdziły to ryzyko. Na przykład w roku 2017 odkryto kilka podatności (m.in. **CVE-2017-2624**, **CVE-2017-2625**, **CVE-2017-2626**), związanych z predykowalnymi (słabo losowymi) kluczami sesji lub atakiem czasowym na porównywanie „cookiów” X11. W skrajnym przypadku podatności te mogły pozwolić lokalnemu atakującemu na przechwycenie i podszycie się pod czyjąś sesję X11, a nawet wykonanie kodu w czyjejś sesji Xorg[bugzilla.redhat.com](https://bugzilla.redhat.com/show_bug.cgi?id=CVE-2017-2624#:~:text=memcmp,Xorg%20session%20of%20another%20user)[nvd.nist.gov](https://nvd.nist.gov/vuln/detail/CVE-2017-2625#:~:text=It%20was%20discovered%20that%20libXdmcp,to%20hijack%20other%20users%27%20sessions). Dlatego w środowiskach produkcyjnych zaleca się wyłączenie ryzykownych usług (np. XDMCP) i używanie SSH Forwarding dla X11.

---

## XDMCP (X Display Manager Control Protocol)

**XDMCP** to starszy protokół zdalnego logowania do X11. Działa na porcie UDP **177** i pozwala przerzucić cały pulpit graficzny (np. Gnome/KDE) z serwera na klienta X11. Mechanizm ten był używany w dawnych systemach z terminalami X – klient X pytał serwer o nową sesję graficzną i otrzymywał przesyłany ekran. Aby zadziałał, na serwerze musi działać menedżer logowania (np. GDM, KDM) skonfigurowany do akceptacji XDMCP.

- **Ryzyko:** XDMCP jest **niezabezpieczony**. Cały ruch (konta użytkowników, hasła, wyświetlane dane) leci w czystym tekście przez UDP[calcomsoftware.com](https://calcomsoftware.com/x-display-manager-control-protocol-xdmcp-explained/#:~:text=Leaving%20XDMCP%20enabled%20has%20several,credentials%2C%20session%20data%2C%20or%20other). Protokół ten jest podatny na ataki typu „man-in-the-middle” – atakujący między klientem a serwerem może podsłuchać lub przechwycić połączenie i przejąć sesję[tenable.com](https://www.tenable.com/plugins/nessus/10891#:~:text=Note%20that%20XDMCP%20is%20vulnerable,keystrokes%20entered%20by%20the%20user). Przykładowo Tenable ostrzega, że XDMCP umożliwia przechwycenie haseł i klawiszy sesji, ponieważ nie używa szyfrowania[tenable.com](https://www.tenable.com/plugins/nessus/10891#:~:text=Note%20that%20XDMCP%20is%20vulnerable,keystrokes%20entered%20by%20the%20user).
    
- **Dostęp:** Jeśli nie potrzebujemy XDMCP, należy go wyłączyć w konfiguracji menedżera sesji (np. w GDM wyłączyć `Enable=false` w `[xdmcp]` pliku `custom.conf`). Pozwala to uniknąć niebezpiecznego nasłuchu na porcie 177.
    

**Podsumowanie:** XDMCP jest starym, niebezpiecznym rozwiązaniem i nie powinien być używany w środowiskach wymagających bezpieczeństwa. W praktyce lepiej stosować bezpieczne protokoły (SSH, TLS, VPN) i nowoczesne narzędzia zdalnego pulpitu.

## Podsumowanie i rekomendacje

- **Windows:** Używaj wbudowanego **RDP** (Remote Desktop) do zdalnego łączenia się z maszynami Windows. Domyślnie RDP wykorzystuje szyfrowanie oraz port TCP 3389[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=By%20default%2C%20RDP%20includes%20encryption%2C,critical%20information%20throughout%20remote%20sessions). Pamiętaj o silnym haśle i ewentualnym ograniczeniu dostępu (np. poprzez VPN).
    
- **Linux:** Do zdalnego GUI można korzystać z **X11 Forwarding** przez SSH (polecenie `ssh -X`), dzięki czemu aplikacje graficzne z serwera wyświetlają się lokalnie. Włącz X11Forwarding w `/etc/ssh/sshd_config` i stosuj tunelowanie SSH, bo X11 samo w sobie nie szyfruje danych[goteleport.com](https://goteleport.com/blog/x11-forwarding/#:~:text=X11%20forwarding%2C%20%60ssh%20,upon%20by%20developers%20for%20securely)[bugzilla.redhat.com](https://bugzilla.redhat.com/show_bug.cgi?id=CVE-2017-2624#:~:text=memcmp,Xorg%20session%20of%20another%20user). Alternatywnie można zainstalować **xrdp** – wtedy Linux działa jak serwer RDP, obsługując połączenia z klienta RDP[github.com](https://github.com/neutrinolabs/xrdp#:~:text=xrdp%20provides%20a%20graphical%20login,a%20variety%20of%20RDP%20clients)[github.com](https://github.com/neutrinolabs/xrdp#:~:text=RDP%20transport%20is%20encrypted%20using,TLS%20by%20default).
    
- **macOS:** macOS oferuje **Screen Sharing/Apple Remote Desktop**, które są oparte na **VNC**. Użytkownicy mogą się łączyć z macOS-em za pomocą klienta VNC. Apple jednak ostrzega, że niektóre aplikacje VNC mogą nie szyfrować połączenia, więc przesyłane dane (zwłaszcza hasła) mogą być podatne na przechwycenie[support.apple.com](https://support.apple.com/guide/remote-desktop/virtual-network-computing-access-and-control-apde0dd523e/mac#:~:text=WARNING%3A%20Allowing%20a%20non,screen%20provides%20nearly%20unrestricted%20access). W razie potrzeby warto również tunelować sesje VNC przez SSH.
    
- **Zabezpieczenia:** Nigdy nie zostawiaj niepotrzebnych usług otwartych (np. XDMCP, niedomyślnie działające serwery VNC). Zawsze stosuj szyfrowanie tunelu (SSH, VPN) do przesyłania ruchu X11/VNC. Sprawdzaj nasłuch serwisów na typowych portach (RDP 3389, X11 6000–6010, VNC 5900+) i ograniczaj ich dostęp w firewallu.
    
- **Nowe technologie:** Tradycyjna architektura X11 powoli ustępuje miejsca Wayland. Wayland domyślnie **nie oferuje** zdalnego renderowania na inny ekran[wayland.freedesktop.org](https://wayland.freedesktop.org/faq.html#:~:text=Is%20Wayland%20network%20transparent%20%2F,does%20it%20support%20remote%20rendering). Zamiast tego stosuje się dedykowane rozwiązania: niektóre kompozytory (np. Weston) mają backend RDP, GNOME udostępnia zdalny pulpit VNC, a projekt **Waypipe** pozwala przenosić pojedyncze aplikacje przez SSH pod Wayland[wayland.freedesktop.org](https://wayland.freedesktop.org/faq.html#:~:text=As%20of%202020%2C%20there%20are,X). Alternatywnie istnieją też aplikacje typu TeamViewer, RustDesk, AnyDesk itp., które zbudowane są na swoich protokołach i szyfrowaniu.
    

Dzięki powyższym protokołom administratorzy mogą elastycznie zarządzać zdalnymi maszynami. Kluczem jest jednak świadomy wybór właściwej technologii i odpowiednie zabezpieczenie połączenia. Przy poprawnym skonfigurowaniu i użyciu nowoczesnych metod, zdalny pulpit może być wygodnym i bezpiecznym narzędziem pracy.

**Źródła:** Opis protokołów RDP/VNC/X11 i ich zabezpieczeń na podstawie dokumentacji Microsoft i RealVNC[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=Remote%20Desktop%20Protocol%20is%20a,network%20managers%20remotely%20diagnose%20problems)[realvnc.com](https://www.realvnc.com/en/blog/what-is-rdp/#:~:text=By%20default%2C%20RDP%20includes%20encryption%2C,critical%20information%20throughout%20remote%20sessions), artykułów o X11 i Wayland[goteleport.com](https://goteleport.com/blog/x11-forwarding/#:~:text=X11%20refers%20to%20the%2011th,workstations%2C%20sometimes%20over%20remote%20networks)[wayland.freedesktop.org](https://wayland.freedesktop.org/faq.html#:~:text=As%20of%202020%2C%20there%20are,X), blogów bezpieczeństwa VNC[pentestpartners.com](https://www.pentestpartners.com/security-blog/vnc-rdp-for-all-to-see/#:~:text=,also%20version%20specific%20exploits%20for) oraz dokumentacji Apple i Linux (SSH, TigerVNC)[support.apple.com](https://support.apple.com/guide/remote-desktop/virtual-network-computing-access-and-control-apde0dd523e/mac#:~:text=WARNING%3A%20Allowing%20a%20non,screen%20provides%20nearly%20unrestricted%20access)[github.com](https://github.com/neutrinolabs/xrdp#:~:text=xrdp%20provides%20a%20graphical%20login,a%20variety%20of%20RDP%20clients).
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)