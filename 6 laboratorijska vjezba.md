# Lab 6: Online and Offline Password Guessing

Na ovim laboratorijskim vjezbama radili smo online i offline napade. Kada se radi o offline napadu, napadac nije u mreznoj interakciji sa serverom.

## Online password guessing

Lozinka se sastojala od 4-6 malih slova. Povezali smo se sa serverom koristeci naredbu 

```jsx
ssh mamic_anamarija@mamicanamarija.local
```

![pwdguessing.jpeg](Lab%206%20Online%20and%20Offline%20Password%20Guessing%2078ba2b4d5db34de78b9a93cd712ba1ca/pwdguessing.jpeg)

Hydra **je alat koji smo koristili za probijanje lozinki, može se koristiti za izvođenje automatiziranih brute force napada na razne protokole. Za nastavak napada potreban je bio dictionary. U terminalu su se “citali” passwordi iz dictionary i hydra je uspjela pogoditi lozinku. S tom lozinkom smo se uspjesno prijavili.

## Offline pasword guessing

Ponovno smo koristili dictionary i usporedivali hash vrijednosti. Kada dode do podudarnosti znaci da je lozinka pogodena. Pokusali smo se prijaviti kao neka druga osoba. Napad je bio uspjesan jer smo imali rijecnik i jer smo znali hash. U konacnici smo se uspjesno prijavili kao Alice Cooper.