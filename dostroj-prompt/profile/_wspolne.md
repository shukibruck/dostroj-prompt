---
status: wspolne
sprawdzono: 2026-09-28
zrodla:
  - https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
  - https://platform.claude.com/docs/en/models/opus-5-5/migration-guide
---

# Reguły wspólne dla modeli

Reguły z ogólnego poradnika Anthropic („Prompting best practices”), który opisuje techniki dla wszystkich obecnych modeli Claude. Przy każdym nowym modelu sprawdź, czy nadal stoją w poradniku. Reguła, która z niego wypadła, idzie do sekcji „Archiwum” na końcu tego pliku, nie do kosza.

Pole `gdzie` mówi, czy reguła cię dotyczy: `czat` (aplikacja Claude), `Claude Code` (terminal, skille, `CLAUDE.md`), `API` (własna aplikacja wołająca Claude).

### W-01 · Bez krzyku przy wyzwalaczach narzędzi i skilli
- gdzie: Claude Code, API
- wykryj: `(You MUST use|MUST use (this|the) (tool|skill)|ALWAYS use (this|the) (tool|skill)|If in doubt, use|MUSISZ użyć|ZAWSZE używaj (narzędzia|skilla))`
- dlaczego: nowsze modele mocniej słuchają system promptu. Język pisany kiedyś przeciw zbyt rzadkiemu używaniu narzędzi („CRITICAL: You MUST use this tool when...”, „If in doubt, use [tool]”) powoduje teraz nadużywanie.
- popraw: zwykłe „Use this tool when...” (po polsku: „Użyj tego narzędzia, gdy...”). Reguła dotyczy wyzwalaczy narzędzi i skilli. Zasad bezpieczeństwa (np. „nigdy nie wysyłaj bez zgody”) nie osłabiaj, patrz tryb `popraw` w `SKILL.md`.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#tool-usage, https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#overthinking-and-excessive-thoroughness

### W-02 · Powód przy instrukcji
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: twarda zasada bez uzasadnienia („NIGDY nie...”, „zawsze...”)`
- dlaczego: kontekst i powód za instrukcją pozwalają modelowi lepiej zrozumieć cel i trafniej ją uogólnić.
- popraw: jedno zdanie „bo...” przy zasadzie. Przykład z poradnika: zamiast `NEVER use ellipses` lepiej:
  ```text
  Your response will be read aloud by a text-to-speech engine, so never use ellipses since the text-to-speech engine will not know how to pronounce them.
  ```
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#add-context-to-improve-performance

### W-03 · Złota zasada: polecenie dla kolegi bez kontekstu
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: polecenie, które zakłada wiedzę, której czytający nie ma (skróty, „jak wcześniej”, nazwy bez wyjaśnienia), albo nie mówi, jaki ma być wynik`
- dlaczego: pokaż polecenie koledze, który prawie nic nie wie o zadaniu. Jeśli on by się pogubił, Claude też się pogubi. Poradnik: konkretny format wyniku i ograniczenia, kroki numerowane, gdy liczy się kolejność albo kompletność. Jeśli chcesz więcej niż minimum, poproś o to wprost.
- popraw: dopisz brakujący kontekst, format wyniku i ograniczenia.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#be-clear-and-direct

### W-04 · Mów, co ma być, zamiast czego ma nie być (format)
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: zakazy formatu bez opisu pożądanego wyniku („nie używaj markdownu”)`
- dlaczego: opis pożądanego wyniku steruje formatem pewniej niż zakaz. Styl polecenia przenosi się na styl odpowiedzi: markdown w poleceniu daje więcej markdownu w wyniku.
- popraw: „Odpowiedź to płynne akapity prozy” zamiast „nie używaj markdownu”. Przy stronach i interfejsach zakazy po nazwie nadal działają (`O55-15`), bo tam chodzi o wybór wzorca wyglądu, nie o format tekstu.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#control-the-format-of-responses

### W-05 · Przykłady: 3-5, różne, w tagach
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: jeden przykład, który model kopiuje zbyt dosłownie, albo przykłady wymieszane z instrukcjami`
- dlaczego: przykłady to jeden z najpewniejszych sposobów sterowania formatem, tonem i strukturą. Mają być trafne, zróżnicowane (inaczej model łapie przypadkowe wzorce) i oddzielone od instrukcji.
- popraw: 3-5 przykładów, każdy w `<example>`, wszystkie razem w `<examples>`.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#use-examples-effectively

### W-06 · Tagi XML oddzielają instrukcje, kontekst i materiał
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: polecenie mieszające instrukcje, kontekst i wklejony materiał bez granic`
- dlaczego: osobny tag na każdy rodzaj treści (`<instructions>`, `<context>`, `<input>`) zmniejsza ryzyko błędnego odczytania. Nazwy tagów spójne i opisowe, zagnieżdżanie przy hierarchii (dokumenty w `<documents>`, każdy w `<document index="n">`).
- popraw: owiń każdy rodzaj treści w swój tag. W aplikacji, do której użytkownicy wklejają tekst z zewnątrz, patrz też `O55-10`.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#structure-prompts-with-xml-tags

### W-07 · Nie wymuszaj raportów co N kroków
- gdzie: Claude Code, API
- wykryj: `(?i)(after every \d+ tool calls|co \d+ (narzędzi|wywołań|kroków))`
- dlaczego: od Opus 4.7 modele same dają regularne aktualizacje w długich przebiegach, więc rusztowanie wymuszające raport co N wywołań narzędzi jest zbędne.
- popraw: usuń. Gdy potrzebny jest przewidywalny rytm (jedno zdanie zapowiedzi przed pierwszym narzędziem, krótkie podsumowanie na końcu), napisz to wprost w system prompcie.
- źródło: https://platform.claude.com/docs/en/models/opus-5-5/migration-guide#behavior-changes, https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5#user-facing-progress-updates

### W-08 · Rola w jednym zdaniu
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: zadanie wymagające konkretnej wiedzy albo tonu, a polecenie nie mówi, w jakiej roli ma działać model`
- dlaczego: ustawienie roli skupia zachowanie i ton modelu na danym zastosowaniu. Nawet jedno zdanie robi różnicę.
- popraw: jedno zdanie roli. Przykład z poradnika:
  ```text
  You are a helpful coding assistant specializing in Python.
  ```
  Poradnik mówi o system prompcie. W zwykłej wiadomości w czacie rolę stawiasz na początku polecenia; to przeniesienie tej zasady, nie osobny test Anthropic.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#give-claude-a-role

### W-09 · Długi materiał na górze, pytanie na końcu
- gdzie: czat, Claude Code, API
- wykryj: `ręcznie: polecenie z długim materiałem (dokument, transkrypcja, dane), w którym pytanie i instrukcje stoją przed materiałem`
- dlaczego: przy dużych materiałach (poradnik: od około 20 tys. tokenów) umieszczenie ich na górze, nad pytaniem i instrukcjami, poprawia wyniki. Pytanie na końcu poprawiało w testach jakość odpowiedzi nawet o 30 procent, szczególnie przy kilku dokumentach. Przy długich dokumentach pomaga też prośba, żeby model najpierw wypisał pasujące cytaty.
- popraw: materiał na górze, każdy dokument w `<document>` z `<source>` i `<document_content>`, wszystkie w `<documents>`. Pytanie i instrukcje na dole. Przy długich dokumentach poproś najpierw o cytaty, np. w tagach `<quotes>`, a dopiero potem o właściwe zadanie.
- źródło: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices#long-context-prompting

## Archiwum

Na razie pusto.
