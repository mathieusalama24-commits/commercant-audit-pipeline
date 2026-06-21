# Pipeline d'audit & refonte de sites de commerçants

Dossier prêt à l'emploi pour Claude Code. Automatise la prospection,
l'audit et la génération de mockups pour des commerçants locaux.

## Installation

1. Si Claude Code n'est pas encore installé :
   ```
   curl -fsSL https://claude.ai/install.sh | sh
   ```
   (Windows PowerShell : `irm https://claude.ai/install.ps1 | iex`)

2. Décompresse ce dossier où tu veux sur ton ordinateur, par exemple :
   ```
   ~/projets/commercant-audit-pipeline/
   ```

3. Ouvre un terminal dans ce dossier et lance :
   ```
   cd ~/projets/commercant-audit-pipeline
   claude
   ```
   Connecte-toi avec ton compte Pro si ce n'est pas déjà fait.

## Utilisation

Une fois dans la session Claude Code, donne simplement une instruction
du type :

```
Lance le pipeline pour plombiers à Lyon, objectif 10 leads.
```

Claude Code va automatiquement :
1. Chercher des plombiers à Lyon
2. Repérer ceux qui ont un site faible ou absent
3. Remplir `leads/leads.csv`
4. Auditer les leads prioritaires (fichiers dans `audits/`)
5. Générer des mockups de refonte pour les meilleurs (fichiers dans
   `mockups/`)
6. Te donner un résumé à la fin

Tu peux ensuite enchaîner avec d'autres villes/secteurs dans la même
session ou une nouvelle : `Lance le pipeline pour boulangeries à
Lyon, objectif 5 leads.`

## Pour ajuster le comportement

Tout le comportement du pipeline est défini dans `CLAUDE.md` — modifie
ce fichier si tu veux changer les critères de priorité, le volume par
session, ou ajouter des étapes (ex : email de prospection automatique
en plus du mockup).

La grille de diagnostic détaillée est dans
`prompts/prompt_audit_site_commercant_v1.txt` — modifie-la si tu veux
affiner les critères d'audit.

## Premiers pas conseillés

Démarre avec un petit volume (5 leads) sur une seule ville/secteur
pour vérifier que la qualité des audits et mockups te convient, avant
de monter en volume. Le fichier CLAUDE.md limite déjà à 10-15 leads
par session par défaut — augmente cette limite seulement si la
qualité reste bonne à plus grande échelle.

## Limite à connaître

La recherche web intégrée à Claude Code n'est pas un outil de
scraping structuré (type Google Maps API) — les résultats de
prospection peuvent être incomplets ou nécessiter une vérification
manuelle, surtout pour les coordonnées de contact. Si tu veux fiabiliser
cette étape, envisage de connecter un serveur MCP dédié à la recherche
de commerces locaux.
