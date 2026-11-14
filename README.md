# TechPhone Accessories : site vitrine WordPress

Projet du module Développement Full Stack et Microservices (ISET Tozeur, AU 2026-2027).

**Réalisé par :** hiba farhat
## Captures d'écran

Site hébergé sur la VM Ubuntu : http://192.168.223.133/phonetech

### Accueil
![Accueil](captures/01-accueil.png)

### Catalogue
![Catalogue](captures/02-catalogue.png)

### À propos
![À propos](captures/03-a-propos.png)

### Blog
![Blog](captures/04-blog.png)

### Contact
![Contact](captures/05-contact.png)

### Administration WordPress
![Admin](captures/06-admin.png)

## 1. Présentation

Site vitrine d'une boutique d'accessoires pour téléphones, réalisé avec WordPress et hébergé sur un serveur Ubuntu (machine virtuelle VMware).

Pages : Accueil, Catalogue (10 produits WooCommerce), À propos, Blog (3 articles minimum), Contact (formulaire + coordonnées).

## 2. Technologies

| Élément | Choix |
|---|---|
| Système | Ubuntu Server (VMware) |
| Serveur web | Apache2 |
| Base de données | MariaDB |
| Langage | PHP |
| CMS | WordPress |
| Thème et template | Astra + Brandstore (Starter Templates) |
| Constructeur de pages | Elementor |
| E-commerce | WooCommerce |
| Formulaire de contact | WPForms |
| SEO | Yoast SEO |

## 3. Installation sur Ubuntu Server

1. Installer la pile LAMP :
   `sudo apt install apache2 mariadb-server php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl libapache2-mod-php unzip -y`
2. Créer la base de données `wordpress_db` et l'utilisateur `wpuser` dans MariaDB.
3. Télécharger WordPress dans `/var/www/html/phonetech` et donner les droits à `www-data`.
4. Activer les permaliens : `sudo a2enmod rewrite` et `AllowOverride All` dans la configuration Apache.
5. Lancer l'installation depuis le navigateur : `http://IP_DE_LA_VM/phonetech`.
6. Migrer le site existant avec le plugin All-in-One WP Migration (export `.wpress` depuis le poste Windows, import sur la VM).
7. Augmenter les limites PHP (`upload_max_filesize`, `post_max_size`) pour l'import.
8. Enregistrer les permaliens (Réglages > Permaliens).

## 4. Contenu du dépôt

- `wp-content/` : thèmes, plugins et médias du site
- `database.sql` : export de la base de données
- `README.md` : ce fichier

Le fichier `wp-config.php` n'est pas versionné : il contient les identifiants de la base.

## 5. Restaurer le site

1. Installer la pile LAMP et WordPress (étapes 1 à 5).
2. Copier `wp-content/` dans `/var/www/html/phonetech/`.
3. Importer la base : `mysql -u wpuser -p wordpress_db < database.sql`
4. Adapter l'adresse du site (options `home` et `siteurl`) si l'IP a changé.

## 6. Architecture WordPress

- **Couche présentation** : thème Astra, template Brandstore, pages construites avec Elementor.
- **Couche métier** : cœur de WordPress en PHP, plugins (WooCommerce, WPForms, Yoast SEO).
- **Couche données** : MariaDB (pages, articles, produits, utilisateurs, réglages) et dossier `wp-content/uploads` pour les médias.
- **Serveur** : Apache reçoit la requête, exécute PHP, qui interroge MariaDB et renvoie la page HTML.

## 7. Monolithique vs microservices

| Critère | WordPress (monolithique) | Microservices / headless |
|---|---|---|
| Structure | Une seule application : affichage, logique et administration ensemble | Services séparés (front, API, base) qui communiquent par API |
| Mise en place | Rapide, peu de code, adapté à un site vitrine | Plus longue, demande plus de développement |
| Évolutivité | Se met à l'échelle ensemble | Chaque service évolue séparément |
| Maintenance | Une seule application, mais les plugins augmentent les risques de conflit | Plus de composants à maintenir et à déployer |
| Technologies | PHP + MySQL imposés | Choix libre par service (par exemple Strapi + Next.js) |
| Panne | Un plugin défaillant peut bloquer tout le site | Une panne peut rester isolée dans un service |

**Conclusion :** pour un site vitrine, WordPress est le choix le plus simple. Une architecture microservices ou headless devient pertinente quand le site grossit ou qu'il faut plusieurs front-ends.

## 8. Difficultés rencontrées

- Erreur de connexion à la base : mot de passe mal saisi et fichier `wp-config.php` incomplet.
- Import limité à 512 Mo : limites PHP et constante `AI1WM_MAX_FILE_SIZE` à relever.
- Menu en double : emplacement du menu mobile vide, affichant toutes les pages.
