# Pipeline d'audit & refonte de sites de commerçants

## Objectif
Automatiser, pour une ville et un type de commerce donnés :
1. La prospection (trouver des commerçants et identifier ceux avec un
   site faible ou absent)
2. L'audit de leur site existant (grille dans prompts/prompt_audit_site_commercant_v1.txt)
3. La génération d'un mockup de site amélioré pour les meilleurs leads

## Commande type pour lancer une session
"Lance le pipeline pour [type de commerce] à [ville], objectif [N] leads."

Exemple : "Lance le pipeline pour plombiers à Lyon, objectif 10 leads."

## Étapes à exécuter, dans l'ordre

### 1. Prospection
- Recherche sur le web des commerçants du type demandé dans la ville
  demandée (utilise WebSearch).
- Pour chaque commerçant trouvé, identifie s'il a un site web. Si oui,
  note l'URL. Si non, ou si le site semble clairement daté/cassé
  (pas de HTTPS, design des années 2000, non responsive), marque-le
  comme "lead prioritaire".
- Ajoute chaque commerçant dans `leads/leads.csv` avec les colonnes :
  `nom,ville,type,url_site,statut_site,telephone,priorite,notes`
  (statut_site = absent / faible / correct / bon ;
   priorite = haute / moyenne / basse)
- Continue jusqu'à atteindre le nombre de leads demandé, ou jusqu'à
  épuisement raisonnable des résultats de recherche (ne pas boucler
  indéfiniment — informe-moi si tu ne trouves pas assez de résultats
  pertinents).

### 2. Audit
- Pour chaque lead avec statut_site = "absent" ou "faible" :
  - Si un site existe, fetch-le et applique la grille de diagnostic
    décrite dans `prompts/prompt_audit_site_commercant_v1.txt`
  - Si aucun site n'existe, fais un diagnostic basé sur ce que tu sais
    du commerce (avis Google si trouvables, secteur, ville) plutôt
    qu'un audit de site
  - Écris le résultat dans `audits/[nom-du-commerce-slugifié].md`

### 3. Mockup
- Pour les leads en priorité "haute" uniquement (sauf si je demande
  explicitement de le faire pour tous) :
  - Génère un mockup HTML complet (un seul fichier, CSS/JS inclus)
    selon la section "ÉTAPE 3 — PROPOSITION DE REFONTE" du prompt
    d'audit
  - Sauvegarde-le dans `mockups/[nom-du-commerce-slugifié].html`

### 4. Récapitulatif
- À la fin d'une session, donne-moi un résumé : nombre de leads
  trouvés, répartition par priorité, et la liste des mockups générés
  avec leur chemin.

## Règles importantes
- Ne jamais reproduire un logo ou une charte graphique existante —
  toujours proposer une direction créative originale inspirée du
  secteur, jamais une copie d'un concurrent identifiable.
- Avant de committer/pousser quoi que ce soit vers un repo Git, me
  demander confirmation explicite.
- Si une recherche ne renvoie pas d'informations de contact fiables
  (téléphone, adresse), le signaler clairement plutôt que d'inventer
  une donnée.
- Rester raisonnable sur le volume par session (10-15 leads max à la
  fois) pour garder un contrôle qualité sur chaque audit/mockup plutôt
  que de produire du volume sans valeur.

## Structure du projet
```
commercant-audit-pipeline/
├── CLAUDE.md                                  # ce fichier
├── prompts/
│   └── prompt_audit_site_commercant_v1.txt     # grille d'audit détaillée
├── leads/
│   └── leads.csv                               # base de prospection, alimentée au fil des sessions
├── audits/                                     # un .md par commerce audité
├── mockups/                                    # un .html par mockup généré
└── README.md                                   # mode d'emploi
```
