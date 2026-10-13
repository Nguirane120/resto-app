# Resto App — commande en ligne pour un restaurant

Application web de commande en ligne pour un restaurant, avec un espace client et un espace d'administration — **prototype d'interface en cours de développement**.

<!-- Ajouter une capture d'écran ici, par exemple : ![Menu](docs/menu.png) -->

## Fonctionnalités

### Espace client
- **Menu** en grille avec photos, prix et badges (populaire, bien noté).
- **Navigation par catégorie** : pizzas, entrées, desserts, boissons.
- **Barre de recherche** de plats.
- **Panier** : récapitulatif de la commande, code promo et frais de livraison.
- **Fiche produit** (composant) : choix de la taille et des suppléments.

### Espace administration
- **Gestion des produits** : tableau triable (nom, image, prix, catégorie) avec ajout et modification dans une fenêtre modale.
- **Barre latérale responsive** et pages prévues pour le tableau de bord, les commandes, les catégories, les clients et les paramètres.

## État du projet

Prototype d'interface : les données sont encore statiques. Prochaines étapes : connexion à une base de données, finalisation des pages d'administration et validation des commandes.

## Stack technique

| Couche | Technologies |
| --- | --- |
| Frontend | Nuxt 3, Vue 3, TypeScript |
| UI | PrimeVue, PrimeIcons, Tailwind CSS |

## Lancer le projet en local

```bash
git clone https://github.com/Nguirane120/resto-app.git
cd resto-app
npm install
npm run dev
```

L'application est disponible sur http://localhost:3000.

## Auteur

**Alioune Badara Nguirane** — Développeur Full Stack Web & Mobile
[GitHub](https://github.com/Nguirane120) · [LinkedIn](https://www.linkedin.com/in/alioune-badara-nguirane-711820197/)
