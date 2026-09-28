# dostroj-prompt

Skill do Claude, który pisze za ciebie polecenia (prompty) pod aktualny model Claude według oficjalnych poradników Anthropic. Mówisz zwykłymi słowami, co chcesz zrobić, a on oddaje gotowe polecenie do skopiowania. Umie też poprawić polecenie, które już masz, i dostroić się do nowego modelu, gdy Anthropic go wypuści.

Zamiast zgadywać, jak ułożyć prośbę, piszesz:

```text
Napisz mi polecenie do streszczenia długiego maila od klienta w 5 punktach.
```

Dostajesz polecenie w ramce do skopiowania, a pod nim listę **Co zastosowałem**. Przy każdej regule jest link do dokumentacji Anthropic, więc możesz sprawdzić ją sam.

## Pobierz

**[dostroj-prompt.zip](https://github.com/shukibruck/dostroj-prompt/releases/latest/download/dostroj-prompt.zip)**

W środku jest folder `dostroj-prompt`, a w nim:
- `SKILL.md`: instrukcja dla Claude, co ma robić;
- `profile/`: reguły spisane z oficjalnej dokumentacji Anthropic, każda z linkiem do źródła.

## Instalacja w aplikacji Claude (czat)

Nazwy przycisków podaję tak, jak w oficjalnej instrukcji Anthropic, czyli po angielsku.

Czego potrzebujesz:
- konta Claude. Według Centrum pomocy Claude skille działają na planach Free, Pro, Max, Team i Enterprise;
- włączonej opcji **Code execution and file creation**. Bez niej skille są wyszarzone.

Kroki:

1. Otwórz [Settings > Capabilities](https://claude.ai/settings/capabilities) i włącz **Code execution and file creation**.
   Plan Team albo Enterprise: tę opcję i opcję **Skills** włącza właściciel organizacji w **Organization settings > Plugins & skills**, zakładka **Policy**. Jeśli nie widzisz przycisku do wgrania skilla, zapytaj go.
2. Otwórz [Customize > Skills](https://claude.ai/customize/skills).
3. Kliknij **+**, a potem **+ Create skill**.
4. Wybierz **Upload a skill** i wskaż plik `dostroj-prompt.zip`. Nie rozpakowuj go wcześniej.
5. Skill pojawi się na liście. Sprawdź, czy przełącznik obok niego jest włączony.

Otwórz nową rozmowę i napisz jedną z próśb z sekcji [Przykłady](#przykłady). Jeśli Claude nie sięgnął po skill, napisz wprost: „Użyj skilla dostroj-prompt i napisz mi polecenie do…”.

Nowa wersja skilla: kliknij go na liście w Customize > Skills, potem **...** obok przełącznika i **Delete**. Następnie wgraj nowy zip tak jak wyżej.

## Instalacja w Claude Code

Czego potrzebujesz:
- zainstalowanego Claude Code;
- konta: subskrypcji Claude Pro, Max, Team albo Enterprise, konta Claude Console albo dostępu przez obsługiwanego dostawcę chmury.

Nie masz Claude Code? Zainstaluj go poleceniem z [oficjalnego przewodnika](https://code.claude.com/docs/en/quickstart):
- Windows (PowerShell): `irm https://claude.ai/install.ps1 | iex`
- macOS i Linux: `curl -fsSL https://claude.ai/install.sh | bash`

Potem otwórz nowe okno terminala i wpisz `claude --version`. Jeśli widzisz numer wersji, wszystko gra.

Kroki:

1. Pobierz i rozpakuj [dostroj-prompt.zip](https://github.com/shukibruck/dostroj-prompt/releases/latest/download/dostroj-prompt.zip) albo sklonuj repo:
   ```
   git clone https://github.com/shukibruck/dostroj-prompt
   ```
2. Przenieś cały folder `dostroj-prompt` do folderu skilli osobistych. Wtedy skill działa we wszystkich twoich projektach.
   - macOS i Linux: `~/.claude/skills/`
   - Windows: `%USERPROFILE%\.claude\skills\` (wklej to w pasek adresu Eksploratora plików; jeśli folderu `skills` nie ma, utwórz go)

   Po przeniesieniu plik instrukcji leży tu: `~/.claude/skills/dostroj-prompt/SKILL.md`. Chcesz skill tylko w jednym projekcie? Włóż folder do `.claude/skills/` w folderze projektu.
3. Uruchom Claude Code poleceniem `claude`. Jeśli był już uruchomiony, a folder `skills` utworzyłeś dopiero teraz, wpisz `/reload-skills` albo uruchom Claude Code ponownie.
4. Wpisz `/skills` i sprawdź, czy `dostroj-prompt` jest na liście.
5. Używaj: wpisz `/dostroj-prompt` i opisz, czego potrzebujesz, albo po prostu napisz „napisz mi polecenie do…”.

Możesz też poprosić Claude Code, żeby zainstalował skill za ciebie:

```text
zainstaluj skill z github.com/shukibruck/dostroj-prompt do moich skilli osobistych
```

Masz już skill w aplikacji Claude? Skille włączone w aplikacji same pobierają się do Claude Code, jeśli logujesz się tym samym kontem Claude. Wymaga to Claude Code w wersji 2.1.273 lub nowszej i nie działa przy logowaniu kluczem API. Zobaczysz je po wpisaniu `/skills`, w grupie **claude.ai sync**.

## Przykłady

**Nowe polecenie.** Opisz zadanie zwykłymi słowami:

```text
Napisz mi polecenie, żeby Claude pomógł mi odpisać klientowi, który pyta, kiedy dostanie zamówienie. Mam dla niego nowy termin i chcę, żeby odpowiedź była krótka i uprzejma.
```

Jeśli czegoś brakuje, skill zada najwyżej 3 pytania i odda jedno polecenie do skopiowania. Pola do uzupełnienia oznaczy nawiasami kwadratowymi, np. `[WKLEJ TU TREŚĆ MAILA]`.

**Poprawka.** Wklej polecenie, którego już używasz:

```text
Popraw ten prompt: [wklej swoje polecenie]
```

Dostaniesz poprawioną wersję i listę zmian, każdą z linkiem do reguły. Jeśli żadna reguła nie pasuje, skill napisze, że nie ma nic do zmiany, zamiast wymyślać poprawki.

**Nowy model.** Gdy Anthropic wypuści kolejny model, podaj link do jego oficjalnego poradnika:

```text
Wyszedł nowy model Claude, oto link do poradnika: [link do dokumentacji Anthropic]
```

Skill przeczyta poradnik, spisze nowy zestaw reguł i pokaże, co się zmieniło. W Claude Code po twojej zgodzie zapisze nowy profil. W aplikacji Claude odda ci plik nowego profilu z instrukcją, jak podmienić skill, bo skilla wgranego do aplikacji nie da się zmienić w rozmowie.

W Claude Code działa jeszcze audyt: „Zrób audyt promptów w tym projekcie”. Skill przejrzy skille i plik `CLAUDE.md`, pokaże tabelę z propozycjami poprawek i niczego nie zmieni bez twojej zgody.

## Skąd są zasady

Każda reguła w skillu ma link do oficjalnej dokumentacji Anthropic. Reguła bez źródła nie wchodzi. Reguły są spisane pod model Claude Opus 5.5 według dokumentacji sprawdzonej 28 września 2026:

- [Poradnik promptowania Claude Opus 5.5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5) i [przewodnik migracji na Opus 5.5](https://platform.claude.com/docs/en/models/opus-5-5/migration-guide): reguły `O55-*`.
- [Poradnik promptowania Claude Opus 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5): reguły `O5-*`. Poradnik Opus 5.5 pisze, że wzorce z poradnika Opus 5 nadal są rozsądnym punktem startu.
- [Ogólny poradnik promptowania Claude (Prompting best practices)](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices): reguły `W-*`, wspólne dla wszystkich obecnych modeli Claude.
- [Dokumentacja Claude Code o modelu i effortcie](https://code.claude.com/docs/en/model-config): ustawianie effortu i słowo `ultrathink` w Claude Code.

Pracujesz na innym modelu niż Opus 5.5? Reguły `W-*` pasują wtedy dalej, a reguły `O55-*` i `O5-*` mogą nie pasować. Skill powie ci to sam i zaproponuje dostrojenie do twojego modelu.

## Gdy coś nie działa

- **Skille są wyszarzone albo nie widzisz sekcji Skills:** wykonywanie kodu jest wyłączone u ciebie (Settings > Capabilities) albo w organizacji.
- **Błąd przy wgrywaniu:** najczęstsze powody według Centrum pomocy to za duży plik, nazwa folderu inna niż nazwa skilla, brak pliku instrukcji albo niedozwolone znaki w nazwie lub opisie. Wgrywaj zip dokładnie taki, jaki pobrałeś, bez zmiany nazw.
- **Claude nie używa skilla:** sprawdź przełącznik w Customize > Skills i poproś wprost o użycie skilla.

Instrukcja instalacji powstała na podstawie:
- [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) (Centrum pomocy Claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills) (Centrum pomocy Claude, struktura zipa)
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills) (Centrum pomocy Claude)
- [Skille w Claude Code](https://code.claude.com/docs/en/skills) (gdzie leżą, synchronizacja z aplikacją)
- [Szybki start Claude Code](https://code.claude.com/docs/en/quickstart) (instalacja, wymagane konto)

---

Robi to [Jacek Kes](https://jacekkes.com). Pokazuję AI dla małych firm po ludzku: [@jacek.kes.ai](https://instagram.com/jacek.kes.ai).
