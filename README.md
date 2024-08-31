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










