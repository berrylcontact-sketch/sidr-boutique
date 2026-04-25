# SIDR — Guide de mise en ligne (15 minutes)

## Ce que vous avez dans ce dossier

```
sidr-project/
├── public/
│   ├── index.html        ← Votre site complet
│   └── merci.html        ← Page affichée après le paiement
├── netlify/
│   └── functions/
│       └── create-checkout.js  ← Le "cerveau" qui parle à Stripe
├── netlify.toml          ← Config Netlify (ne pas modifier)
├── package.json          ← Dépendances (ne pas modifier)
├── .env.example          ← Template pour vos clés secrètes
└── README.md             ← Ce guide
```

---

## Étape 1 — Récupérez votre clé Stripe (2 min)

1. Allez sur https://dashboard.stripe.com
2. Menu gauche → **Développeurs** → **Clés API**
3. Copiez la **Clé secrète** (commence par `sk_live_` ou `sk_test_`)

---

## Étape 2 — Créez un compte GitHub et uploadez le projet (5 min)

> GitHub est gratuit et nécessaire pour connecter Netlify.

1. Allez sur https://github.com et créez un compte
2. Cliquez **"New repository"** → nommez-le `sidr-boutique` → **Create**
3. Uploadez tous les fichiers de ce dossier via l'interface web de GitHub
   (bouton **"uploading an existing file"** sur la page du repo)

---

## Étape 3 — Déployez sur Netlify (5 min)

1. Allez sur https://netlify.com et connectez-vous avec GitHub
2. Cliquez **"Add new site"** → **"Import an existing project"**
3. Choisissez votre repo `sidr-boutique`
4. Laissez les paramètres par défaut → **Deploy**

Votre site est en ligne ! Vous obtenez une URL du type `sidr-boutique-xxx.netlify.app`

---

## Étape 4 — Ajoutez votre clé Stripe sur Netlify (2 min)

⚠️ Ne mettez JAMAIS votre clé directement dans le code — c'est dangereux.
Netlify stocke vos clés de façon sécurisée.

1. Dans votre dashboard Netlify → **Site configuration** → **Environment variables**
2. Cliquez **"Add a variable"**
3. Ajoutez ces deux variables :

| Clé | Valeur |
|-----|--------|
| `STRIPE_SECRET_KEY` | `sk_live_VOTRE_CLE_ICI` |
| `SITE_URL` | `https://VOTRE_SITE.netlify.app` |

4. Cliquez **Save** puis **Trigger deploy** (redéployez le site)

---

## Étape 5 — Testez le paiement

1. Allez sur votre site → cliquez **Acheter** sur un produit
2. Vous êtes redirigé vers une page Stripe sécurisée
3. En mode test, utilisez la carte : `4242 4242 4242 4242` (n'importe quelle date future, n'importe quel CVC)
4. Après paiement → vous arrivez sur votre page "Merci"
5. Dans votre dashboard Stripe → **Paiements** → vous voyez la commande ✓

---

## Étape 6 (optionnel) — Votre propre domaine

1. Achetez un domaine sur https://www.ovh.com (ex: `sidr-france.fr` ≈ 10€/an)
2. Dans Netlify → **Domain management** → **Add domain**
3. Suivez les instructions pour pointer votre domaine vers Netlify
4. N'oubliez pas de mettre à jour `SITE_URL` dans vos variables d'environnement

---

## Personnaliser votre site

### Changer les prix
Dans `public/index.html`, cherchez et modifiez :
- `12,90 €` → votre prix 100g
- `26,90 €` → votre prix 250g
- `44,90 €` → votre prix 500g

Et dans `netlify/functions/create-checkout.js` :
- `price: 1290` → votre prix en centimes (12,90€ = 1290)
- `price: 2690` → idem pour 250g
- `price: 4490` → idem pour 500g

### Changer le nom de la marque
Dans `public/index.html`, cherchez `SIDR` et remplacez par votre nom.

### Changer les pays de livraison
Dans `create-checkout.js`, la ligne `allowed_countries` liste les pays acceptés.
Codes pays : FR=France, BE=Belgique, CH=Suisse, MA=Maroc, DZ=Algérie, TN=Tunisie

### Ajouter vos vraies photos
1. Placez vos photos dans `public/images/`
2. Dans `create-checkout.js`, décommentez la ligne `// images: [produit.image]`
3. Mettez à jour les URLs d'images avec votre vrai nom de domaine

---

## Basculer en mode "live" (vrai argent)

Quand vous êtes prêt à recevoir de vrais paiements :
1. Stripe → Développeurs → désactivez le "Mode test"
2. Copiez la nouvelle clé `sk_live_...`
3. Mettez à jour `STRIPE_SECRET_KEY` dans Netlify
4. Redéployez

---

## Support

Des questions ? Vous pouvez toujours demander à Claude de vous aider !
