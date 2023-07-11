---
layout: post
title: Hur kan man fixa felaktig comitter i git
categories: [Git,2. Svenska]
tags: [Git, Gitlab, Github, Commit]
---

När du behöver korrigera författarinformationen i ditt Git-repositorium, följ dessa steg:

**Observera**: Att ändra commit-historik med hjälp av `git filter-branch` och att tvinga igenom ändringar kan få betydande konsekvenser. Det är viktigt att vara försiktig och förstå konsekvenserna innan du fortsätter.

Att använda `git filter-branch` och tvinga igenom ändringar kan få flera konsekvenser:

- **Förlust av commit-historik**: Att ändra commit-information skapar nya commits med annan metadata, vilket i praktiken ersätter de gamla commitsen och skriver om commit-historiken. Det kan bli svårt att spåra de ursprungliga commitsen och tillhörande information.

- **Samarbetsproblem**: Att skriva om commit-historiken kan orsaka synkroniseringsproblem när flera personer samarbetar på samma kodbas. Andra medarbetare kan ha utgått från de ursprungliga commitsen, vilket kan leda till konflikter och svårigheter att sammanfoga deras arbete med den uppdaterade historiken.

- **Störningar av integrationer**: Att ändra commit-historiken kan störa integrationer med olika verktyg och tjänster som kontinuerlig integration (CI)-pipelines, ärendehanterare och kodgranskningssystem. Dessa integrationer förlitar sig på de ursprungliga commitsen och deras metadata för korrekt funktion.

För att fixa felaktig författarinformation, följ dessa steg:

1. Öppna din terminal och kör följande kommando:
```bash
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="<felaktig_epost>"
CORRECT_NAME="<korrekt_namn>"
CORRECT_EMAIL="<korrekt_epost>"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

2. Efter att ha kört kommandot, kör följande kommando för att tvinga igenom de uppdaterade commitsen och taggarna till ditt fjärrrepository:
```bash
git push --force --tags origin 'refs/heads/*'
```

Genom att följa dessa steg kommer du att fixa felaktig författarinformation i ditt Git-repositorium. Se till att ersätta `<felaktig_epost>`, `<korrekt_namn>` och `<korrekt_epost>` med lämpliga värden som är specifika för din situation.