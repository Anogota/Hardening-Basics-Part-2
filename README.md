# Hardening-Basics-Part-2

Hej,
Zademonstuje jak można rozwiązać pokój na TryHackMe - Hardening-Basics-Part-2
https://tryhackme.com/r/room/hardeningbasicspart2

Zabierajmy się do roboty.

  1.Introduction
W tym pokoju zostaną omówione nastepujące rzeczy:
SSH i szyfrowanie (rozdział 3)
Obowiązkowa kontrola dostępu (rozdział 4)

Możesz uzyskać dostęp do maszyny za pomocą następujących danych uwierzytelniających
spooky:tryhackme

Wszystkie zadania w tym pokoju zostały wykonane przy użyciu Ubuntu 18.04 LTS. To powiedziawszy, prawie wszystko, co dotyczy 18.04, może również dotyczyć 20.04

Task 1:
Deploy the VM if necessary and let's go!

No answer needed


  2.~~~~~ Chapter 3 Quiz ~~~~~

Task 1: Która wersja protokołu SSH jest najbezpieczniejsza?
![image](https://github.com/user-attachments/assets/1add8a93-b4f6-4054-b0ed-e054f522d5c6)

Jest to odpowiedź 2.

Task 2:Jest to losowa, dowolna liczba, używana jako klucz sesji, który służy do szyfrowania GPG.
Jest to: nonce

Task 3:GPG opiera się na standardzie OpenGPG
![image](https://github.com/user-attachments/assets/771e52d1-c574-49fb-8fca-1064df8a6dac)

Odpowiedźią na te pytanie jest yey

Task 4: Jakie polecenie służy do generowania kluczy GPG?

Tutaj odpowiedź znalazłem za pomocą komendy man gpg:
![image](https://github.com/user-attachments/assets/260ad292-443a-4de2-8c9f-901d54e16dff)

Task 5: Jakie polecenie służy do symetrycznego szyfrowania pliku za pomocą GPG
![image](https://github.com/user-attachments/assets/3a02b867-bcb6-4a86-896b-ca896b25b7e0)
Odpowiedźią na te pytanie gpg -c

Task 6: Jakie polecenie służy do asymetrycznego szyfrowania pliku za pomocą GPG?
![image](https://github.com/user-attachments/assets/f520a1bf-d8b9-4db9-ae96-3e349860f9b8)
Odpowiedźią jest gpg -e

Task 7:Jakie jest polecenie do tworzenia kluczy SSH?
Odpowiedźią na te pytanie jest: SSH-KEYGEN

Task 8: Gdzie w katalogu domowym użytkownika przechowywane są klucze SSH?
Są przechowywani w .ssh

Task 9: Jaką opcję należy ustawić, aby wybrać typ klucza, który ma zostać wygenerowany dla protokołu SSH?
Jest to opcja: -t

Task 10: W jakim pliku znajdują się opcje konfiguracji SSH przedstawione w tym rozdziale (pełna ścieżka)?
Jest to /etc/ssh/sshd_config
![image](https://github.com/user-attachments/assets/272ff169-cb9b-4cfa-bf0c-d6a16a47ad06)


  3.Ochrona prywatności GNU
Korzystanie z GPG
Kiedy po raz pierwszy chcesz użyć GPG, wymaga to utworzenia własnych kluczy. Aby to zrobić, używamy  gpg --gen-key.
Ten proces jest niezwykle prosty. Po naciśnięciu Enter po wprowadzeniu powyższego polecenia, zostaną utworzone pliki i katalogi, a także poproszone o podanie informacji:
Utworzyłem sobie na test takowy klucz, patrz poniżej:
![image](https://github.com/user-attachments/assets/cc1ea152-93c1-4e70-b170-528686f8df12)

Po pewnym czasie proces się zakończy i utworzysz kolejny katalog w swoim katalogu domowym o nazwie .gnugpg

  4.Szyfrowanie plików
Możemy zaszyfrować nasz plik aby nikt go nie mógł odycztać za pomocą hasła! Zrobimy to za pomocą  gpg -c <our_file> 
Możemy również odszyfrować nasz plik za pomocą -d, podając hasło.
Spowoduje to wydrukowanie zawartości pliku po wyświetleniu monitu o podanie tajnego hasła.

  5.Protokół SSH 1
W swoim  /etc/ssh/sshd_config pliku, jeśli widzisz 

Protocol 1
Lub 
Protocol 1, 2
powinieneś wyłączyć Protokół w wersji 1 tak szybko, jak to możliwe. Jest dostępny do uruchomienia na maszynach Legacy, ale został naruszony i nie jest już uważany za bezpieczny. Protokół SSH w wersji 2 jest obecną, bezpieczniejszą wersją SSH .

Akruatnie w naszym przypadku OpenBSD, protocol 2 jest domyślnie włączony dla nowszych wersji.

  6.Tworzenie zestawu kluczy SSH
Kopiowanie za pomocą ssh-copy-id
Najłatwiejszym sposobem na skopiowanie kluczy ssh jest użycie prostego polecenia  ssh-copy-id. Wszystko, co musisz zrobić, to  ssh-copy-id username@remote-host odpowiedzieć na pytania i gotowe

Kopiowanie ręczne:
Kopiowanie za pomocą ssh-copy-id
Najłatwiejszym sposobem na skopiowanie kluczy ssh jest użycie prostego polecenia  ssh-copy-id. Wszystko, co musisz zrobić, to  ssh-copy-id username@remote-host odpowiedzieć na pytania i gotowe

Tworzenie kluczy z zaktualizowanymi algorytmami szyfrowania
by utworzyć zmodyfikowany klucz RSA, możemy użyć następującego polecenia podczas generowania klucza
ssh-keygen -t rsa -b 3072
Opcja  -t określa typ szyfrowania, a  -b opcja określa rozmiar bitowy.  

W tym przypadku wybraliśmy opcje -b 3072 ponieważ:
Narodowy Instytut Norm i Technologii USA ( NIST ) zaleca obecnie stosowanie algorytmu RSA o długości co najmniej 3072 bitów lub klucza algorytmu podpisu cyfrowego opartego na krzywych eliptycznych (ECDSA) o długości co najmniej 384 bitów.

  7.Wyłącz nazwę użytkownika i hasło logowania SSH
Możemy to edytować w tym pliku: /etc/ssh/sshd_config
W naszej maszynie mamy tą opcję włączoną
![image](https://github.com/user-attachments/assets/20531b67-3866-471e-ae72-401684b9f0dc)

Poniewaz jeślibyłaby ta wyłączona ryzykowalibyśmy zablokowaniem siebie oraz innych użytkowników, ta opcja to tylko i wyłącznie dobra w momencie kiedy nasze logowanie odbywa sie za pomocą kluczy.

  8.Przekierowanie X11 i tunelowanie SSH
Wyłączenie przekierowania X11 jest bardzo proste. To kolejne ustawienie w  sshd_config. Będziesz chciał znaleźć linię, która mówi
X11 Forwarding yes    
i zmień to na „no”
X11 pozwala przekazywać wyświetlacze aplikacji GUI do lokalnego środowiska (chociaż musi mieć samo GUI, prawda?). Jednak X11 ma pewne wady, które sprawiają, że korzystanie z niego jest niebezpieczne.

  9.Ulepszanie rejestrowania SSH
Plik dziennika jest tworzony za każdym razem, gdy ktoś loguje się przy użyciu protokołu, który używa SSH. Tak więc będzie to SSH, SCP lub SFTP. Domyślnie Ubuntu przechowuje ten plik dziennika w  /var/log/auth.log. Wygląda on mniej więcej tak
Tutaj musiałem się posłużyc screanem z tryhackme, ponieważ użytkownik spooky nie nalezy do grupy sudoers:
![image](https://github.com/user-attachments/assets/cfecd35a-4646-4c35-8332-a716edea1878)

Przejdź do  /etc/ssh/sshd_config i poszukaj wiersza, który mówi 
#LogLevel INFO
Możemy odkomentować ten wiersz i zmienić go na dowolny z dostępnych poziomów powyżej. I tak po prostu, teraz zobaczysz bardziej szczegółowe dzienniki w pliku  /var/log/auth.log .

  10.~~~~~Obowiązkowa kontrola dostępu ~~~~~
﻿Mandatory Access Control (MAC) to rodzaj kontroli dostępu. Jest ona łączona z Discretionary Access Control, Role Based Access Control i Rule Based Access Control. MAC jest uważany za najsilniejszą formę kontroli dostępu, ponieważ umożliwia większą kontrolę nad tym, kto ma dostęp do czego. W systemie Linux istnieje wiele sposobów na zaimplementowanie MAC. Dwa z nich to SELinux i AppArmor. W tym rozdziale przyjrzymy się AppArmor i zobaczymy, jak administrator systemu lub entuzjasta bezpieczeństwa może wzmocnić swoje systemy za pomocą MAC. 

  11.Wprowadzenie do AppArmor
Aplikacja Armor

*Może uniemożliwić złośliwym aktorom dostęp do danych w Twoich systemach. Jako administrator systemu, jest to niezwykle ważne; ochrona poufności Twoich danych
*Aplikacje mają własne profile, co ułatwia sprawę
*SELinux i AppArmor umożliwiają tworzenie własnych profili, ale skrypty w AppArmor są nieco łatwiejsze do zrozumienia i skracają czas nauki

Katalog AppArmor znajduje się w etc/apparmor.d w tym katalogu możemy znaleźc wszystkie profile AppArmor między innymi: sbin.dhclient i  usr.*
![image](https://github.com/user-attachments/assets/62738c1a-03a8-43e3-ad69-f58a2a7bc8c5)

Katalog  abstractions jest rodzajem folderu „includes”, który zawiera częściowo napisane profile, które można wykorzystać i uwzględnić we własnych profilach. Część pracy została już wykonana za Ciebie. Oto lista  abstractions katalogu

![image](https://github.com/user-attachments/assets/44b55c77-a08e-4a8c-adaa-3647986fac80)

  12.Narzędzia wiersza poleceń AppArmor
Aby uzyskać status AppArmor, możemy wpisać  aa-status. Daje nam to dość długi wynik.
Tutaj ponownie musiałem się posłużyć screanem z tryhackme, ponieważ nie miałem wystarczających uprawień aby wykonać te polecenie:
![image](https://github.com/user-attachments/assets/28795191-3890-45f4-bce1-98916dfbf5de)

Widzimy, że AppArmor jest rzeczywiście załadowany i obecnie jest ustawiony na tryb wymuszania. Załadowano 19 profili. To ładnie przechodzi do różnych trybów AppArmor.

Wymuś – wymusza aktywne profile
Zgłoś skargę – umożliwia procesom wykonywanie niedozwolonych przez profil działań i jest rejestrowana
Audyt – taki sam jak tryb wymuszania, ale dozwolone i niedozwolone działania są rejestrowane w  /var/log/audit/audit.log dzienniku systemowym (w zależności od tego, czy  auditd jest zainstalowany).

  13.~~~~~Quiz ~~~~~

Task 1: Gdzie znajdują się profile AppArmor?
etc/apparmor.d - możemy to zobaczyć powyzej w writeup, jest to tam bardziej szczegółowo omówione.

Task 2:Ten katalog zawiera częściowe profile, które można wykorzystać we własnych profilach niestandardowych
Odpowiedźią na te pytanie jest: abstraction

Task 3:Ten znak interpunkcyjny jest WYMAGANY na końcu każdej reguły w profilu
Jest to: .

Task 4: Ten tryb AppArmor wymusza stosowanie profili, ale także je rejestruje
Odpowiedźią jest: audit - Audyt – taki sam jak tryb wymuszania, ale dozwolone i niedozwolone działania są rejestrowane w  /var/log/audit/audit.log dzienniku systemowym 

Task 5:To polecenie sprawdza stan AppArmor
Odpowiedźią jest: aa-status - również o tym wspomnieliśmy powyżej.

  14.~~~~~SSH i szyfrowanie ~~~~~
Szyfrowanie obejmuje wiele różnych rzeczy. Tylko w samym Sec+ obejmujesz szyfrowanie symetryczne i asymetryczne, klucze prywatne i publiczne, różne szyfry, haszowanie... to dużo. To będzie mniej niż to i bardziej skupione na szyfrowaniu w odniesieniu do Ubuntu i wzmacniania systemu/serwera. Więc usiądź wygodnie i przygotujmy się. Omówimy tutaj wiele rzeczy. Tematy będą obejmować:

Ochrona prywatności GNU
Szyfrowanie plików za pomocą GPG
SSH
Wyłączanie protokołu SSH 1
Tworzenie kluczy
Wyłączanie logowania za pomocą nazwy użytkownika i hasła
Konfigurowanie szyfrowania SSH
Bardziej szczegółowe rejestrowanie
Wyłączanie tunelowania SSH
Wyłączanie przekazywania X1

Dziękuje wszystkim, właśnie ukończyliśmy pokój.

