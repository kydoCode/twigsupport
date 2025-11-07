# Activité de remédiation Twig : Héritage et Layouts

## Objectifs pédagogiques

- Comprendre le fonctionnement de Twig dans Symfony
- Identifier les éléments communs entre plusieurs pages HTML
- Créer un template parent (`base.html.twig`) et des templates enfants (`home.html.twig`, `about.html.twig`)
- Apprendre à utiliser `extends`, `block` et `include`
- Introduire la notion de layout (sous-base) et la fonction `{{ parent() }}`

## Structure du projet

```
templates/
├── base.html.twig
├── home.html.twig
├── about.html.twig
├── partials/
│   ├── _header.html.twig
│   └── _footer.html.twig
└── admin/
    └── layout.html.twig
```

## Étapes de réalisation

### Étape 1 – Structure HTML de départ

Créer deux pages statiques :
- `home_static.html`
- `about_static.html`

Structure de base :
```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Accueil</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
<header>
      <a href="/">
        <img src="images/logo.png" alt="Logo du site">
      </a>
      <nav>
        <ul>
          <li><a href="home_static.html">Accueil</a></li>
          <li><a href="about_static.html">À propos</a></li>
        </ul>
      </nav>
    </header>

  <main>
    <h1>Bienvenue sur la page d'accueil</h1>
    <p>Ceci est une simple page HTML de départ.</p>
  </main>

  <footer>
    <p>© 2025 - Mon site Twig</p>
  </footer>

  <script src="script.js"></script>
</body>
</html>
```

### Étape 2 – Créer le template parent

Créer `templates/base.html.twig` :

```twig
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>{% block title %}Titre par défaut{% endblock %}</title>
  <link rel="stylesheet" href="{{ asset('style.css') }}">
</head>
<body>
  
   {% block header %}
    <header>
      <a href="/">
        <img src="{{ asset('images/logo.png') }}" alt="Logo du site">
      </a>
      <nav>
        <ul>
          <li><a href="/home_static.html">Accueil</a></li>
          <li><a href="/about_static.html">À propos</a></li>
        </ul>
      </nav>
    </header>
  {% endblock %}

  {% block body %}{% endblock %}

  {% block footer %}
    <footer>
      <p>© 2025 - Mon site Twig</p>
    </footer>
  {% endblock %}
</body>
</html>
```

### Étape 3 – Créer les templates enfants

**home.html.twig**
```twig
{% extends 'base.html.twig' %}

{% block title %}Accueil{% endblock %}

{% block body %}
  <main>
    <h1>Bienvenue sur la page d'accueil</h1>
    <p>Ceci est le contenu du template enfant.</p>
  </main>
{% endblock %}
```

**about.html.twig**
```twig
{% extends 'base.html.twig' %}

{% block title %}À propos{% endblock %}

{% block body %}
  <main>
    <h1>À propos de ce site</h1>
    <p>Voici la page de présentation.</p>
  </main>
{% endblock %}
```

### Étape 4 – Séparer les composants communs

Créer les fichiers partiels :
- `templates/partials/_header.html.twig`
- `templates/partials/_footer.html.twig`

Puis dans `base.html.twig` :
```twig
{% block header %}
  {% include 'partials/_header.html.twig' %}
{% endblock %}

{% block footer %}
  {% include 'partials/_footer.html.twig' %}
{% endblock %}
```

### Étape 5 – Créer un layout secondaire

Créer `templates/admin/layout.html.twig` :

```twig
{% extends 'base.html.twig' %}

{% block header %}
  {{ parent() }}
  <nav class="admin-nav">Section admin</nav>
{% endblock %}

{% block body %}
  <main>
    <h1>Espace Administration</h1>
    {% block admin_content %}{% endblock %}
  </main>
{% endblock %}
```

## Syntaxe Twig - Référence rapide

| Action | Syntaxe |
|--------|---------|
| Étendre un template | `{% extends 'base.html.twig' %}` |
| Définir un bloc | `{% block name %}...{% endblock %}` |
| Inclure un sous-template | `{% include 'partials/_header.html.twig' %}` |
| Appeler le contenu du parent | `{{ parent() }}` |
| Variable | `{{ variable }}` |
| Condition | `{% if ... %} ... {% endif %}` |
| Boucle | `{% for item in list %} ... {% endfor %}` |

## Notes importantes

- Ne pas modifier le `base.html.twig` généré automatiquement par Symfony
- S'inspirer du template de base pour créer ses propres templates
- Chaque page spécifique devient une vue enfant du template parent