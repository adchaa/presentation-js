---
theme: geist
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
mdc: true
info: |
  ## Événements JavaScript et Manipulation du DOM
---

# Événements JavaScript et Manipulation du DOM
## Construire des applications web interactives

---
layout: two-cols
---

# Ce Que Nous Allons Couvrir

<div class="text-left">

- JavaScript et le DOM
- Comprendre les Événements
- Gestion des Événements
- Propagation des Événements
- Événements DOM Courants
- Manipulation du DOM
- Exemples Pratiques
- Bonnes Pratiques

</div>

::right::

<div class="flex justify-center h-full items-center">
  <div class="w-40 h-40 bg-yellow-300 rounded-lg shadow-xl flex items-center justify-center transform hover:rotate-45 transition-transform duration-500 cursor-pointer">
    <span class="text-xl font-bold">Survolez-moi !</span>
  </div>
</div>

---

# Mouse Hover

<div id="mouse"  class="flex justify-center items-center border-2 rounded w-40 h-40">
  déplacer le curseur 
</div>

```js
  const mouse = document.getElementById("mouse")
  mouse.addEventListener("mousemove",(e)=>{
    mouse.textContent = `(${e.x},${e.y})`
  })
```

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const mouse = document.getElementById("mouse")
  mouse.addEventListener("mousemove",(e)=>{
    mouse.textContent = `(${e.x},${e.y})`
  })
})
</script>

---

# Démo : Écouteurs d'Événements

<button id="demo-button" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Cliquez-moi
</button>

```js
const bouton = document.getElementById('demo-button');

bouton.addEventListener('click', function() {
  bouton.style.backgroundColor = getRandomColor()
});
```

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  function getRandomColor() {
    var letters = '0123456789ABCDEF';
    var color = '#';
    for (var i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }
  const bouton = document.getElementById('demo-button');

  bouton.addEventListener('click', function() {
    bouton.style.backgroundColor = getRandomColor()
  });
})
</script>

---
layout: two-cols
---

# Propagation des Événements

Les événements dans le DOM remontent de l'élément cible jusqu'à la racine.

<div class="mt-4">
  <h3 class="text-yellow-500">Trois Phases :</h3>
  <ol>
    <li>Phase de Capture</li>
    <li>Phase de Cible</li>
    <li>Phase de Bouillonnement</li>
  </ol>
</div>

::right::

<div class="flex items-center justify-center h-full">
  <div class="relative border-4 border-blue-500 p-8 w-64 h-64 rounded-lg">
    <div class="text-center">Document</div>
    <div class="absolute top-12 left-12 right-12 bottom-12 border-4 border-green-500 rounded-lg p-4">
      <div class="text-center">Parent</div>
      <div class="absolute top-12 left-8 right-8 bottom-8 border-4 border-red-500 rounded-lg flex items-center justify-center">
        <div class="text-center">Cible</div>
      </div>
    </div>
  </div>
</div>

---

# Méthodes de Gestion des Événements

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-xl text-yellow-500 mb-2">Méthode 1 : Attributs HTML</h3>
```html
<button onclick="gererClic()">Cliquez-moi</button>
```
    <div class="mt-2 text-red-500">⚠️ Non recommandé - mélange HTML et JS</div>
  </div>
  <div>
    <h3 class="text-xl text-yellow-500 mb-2">Méthode 2 : Propriétés DOM</h3>
```js
const bouton = document.querySelector('button');
bouton.onclick = function() {
  alert('Bouton cliqué !');
};
```
    <div class="mt-2 text-orange-500">⚠️ Limité - un seul gestionnaire par événement</div>
  </div>
</div>

<div class="mt-6">
  <h3 class="text-xl text-yellow-500 mb-2">Méthode 3 : addEventListener (Recommandée)</h3>
```js
const bouton = document.querySelector('button');
bouton.addEventListener('click', function() {
  alert('Premier gestionnaire');
});
bouton.addEventListener('click', function() {
  alert('Deuxième gestionnaire');
});
```
  <div class="mt-2 text-green-500">✓ Gestionnaires multiples, plus de contrôle, séparation plus propre</div>
</div>

---

# Démo Interactive : Propagation d'Événements

<div class="grid grid-cols-2 gap-8">
  <div>
    <div id="parent" class="bg-blue-200 p-8 rounded-lg relative">
      <div class="text-center mb-4">Élément Parent</div>
      <div id="child" class="bg-green-200 p-6 rounded-lg">
        <div class="text-center mb-2">Élément Enfant</div>
        <button id="btn" class="bg-red-500 text-white px-4 py-2 rounded">Cliquez-moi</button>
      </div>
      <div id="event-log" class="mt-4 text-sm h-32 overflow-y-auto bg-gray-100 p-2 rounded"></div>
    </div>
  </div>
  <div>
```js
const parent = document.getElementById('parent');
const enfant = document.getElementById('child');
const bouton = document.getElementById('btn');
const journal = document.getElementById('event-log');

function enregistrerEvenement(evenement) {
  const entree = document.createElement('div');
  entree.textContent = `${evenement.type} sur ${evenement.currentTarget.id}`;
  journal.prepend(entree);
}

parent.addEventListener('click', enregistrerEvenement);
enfant.addEventListener('click', enregistrerEvenement);
bouton.addEventListener('click', enregistrerEvenement);
```
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const parent = document.getElementById('parent')
  const enfant = document.getElementById('child')
  const bouton = document.getElementById('btn')
  const journal = document.getElementById('event-log')
  
  if (parent && enfant && bouton && journal) {
    function enregistrerEvenement(evenement) {
      const entree = document.createElement('div')
      entree.textContent = `${evenement.type} sur ${evenement.currentTarget.id}`
      journal.prepend(entree)
    }
    
    parent.addEventListener('click', enregistrerEvenement)
    enfant.addEventListener('click', enregistrerEvenement)
    bouton.addEventListener('click', enregistrerEvenement)
  }
})
</script>

---

# Manipulation du DOM

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Sélection d'Éléments</h3>
```js
// Par ID
const element = document.getElementById('monId');

// Par nom de classe
const elements = document.getElementsByClassName('maClasse');

// Par nom de balise
const divs = document.getElementsByTagName('div');

// Sélecteurs CSS
const premierPara = document.querySelector('p.intro');
const tousLesLiens = document.querySelectorAll('a.externe');
```
  </div>
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Modification d'Éléments</h3>
```js
// Changer le contenu
element.textContent = 'Nouveau texte';
element.innerHTML = '<strong>Texte en gras</strong>';

// Modifier les attributs
element.setAttribute('class', 'surligne');
element.id = 'nouvelId';

// Changer les styles
element.style.color = 'red';
element.style.backgroundColor = '#f0f0f0';
```
  </div>
</div>

---

# Création et Suppression d'Éléments

<div class="grid grid-cols-2 gap-4">
<div>
  <h3 class="text-yellow-500 text-xl mb-2">Création d'Éléments</h3>
```js
// Créer un nouvel élément
const nouvDiv = document.createElement('div');

// Ajouter du contenu
nouvDiv.textContent = 'Je suis une nouvelle div !';

// Ajouter du style
nouvDiv.className = 'nouvel-element';

// Ajouter au DOM
document.body.appendChild(nouvDiv);
```
</div>


<div>
  <h3 class="text-yellow-500 text-xl mb-2">Suppression d'Éléments</h3>
```js
// Méthode 1 : Supprimer directement
element.remove();

// Méthode 2 : Supprimer du parent
const parent = element.parentNode;
parent.removeChild(element);

// Effacer tous les enfants
while (parent.firstChild) {
  parent.removeChild(parent.firstChild);
}
```
</div>
</div>

---

# Démo Interactive : Manipulation du DOM

<div class="grid grid-cols-2 gap-8">
  <div>
    <div id="todo-app" class="w-full max-w-md mx-auto bg-white rounded-lg shadow-md overflow-hidden p-4">
      <h2 class="text-center text-xl font-bold mb-4">Liste de Tâches</h2>
      <div class="flex mb-4">
        <input id="todo-input" type="text" class="flex-1 border rounded-l px-4 py-2" placeholder="Ajouter un nouvel élément...">
        <button id="add-button" class="bg-blue-500 text-white px-4 py-2 rounded-r">Ajouter</button>
      </div>
      <ul id="todo-list" class="divide-y"></ul>
    </div>
  </div>
  <div>
```js
const saisie = document.getElementById('todo-input');
const boutonAjouter = document.getElementById('add-button');
const listeTaches = document.getElementById('todo-list');

boutonAjouter.addEventListener('click', function() {
  if (saisie.value.trim() === '') return;
  
  // Créer un élément de liste
  const li = document.createElement('li');
  li.className = 'py-2 flex justify-between items-center';
  
  // Créer le texte et le bouton de suppression
  li.innerHTML = `
    <span>${saisie.value}</span>
    <button class="delete-btn text-red-500">×</button>
  `;
  
  // Ajouter la fonctionnalité de suppression
  li.querySelector('.delete-btn').addEventListener('click', 
    function() {
      li.remove();
    });
  
  // Ajouter à la liste et effacer la saisie
  listeTaches.appendChild(li);
  saisie.value = '';
});
```
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const saisie = document.getElementById('todo-input')
  const boutonAjouter = document.getElementById('add-button')
  const listeTaches = document.getElementById('todo-list')
  
  if (saisie && boutonAjouter && listeTaches) {
    boutonAjouter.addEventListener('click', function() {
      if (saisie.value.trim() === '') return
      
      // Créer un élément de liste
      const li = document.createElement('li')
      li.className = 'py-2 flex justify-between items-center'
      
      // Créer le texte et le bouton de suppression
      li.innerHTML = `
        <span>${saisie.value}</span>
        <button class="delete-btn text-red-500">×</button>
      `
      
      // Ajouter la fonctionnalité de suppression
      li.querySelector('.delete-btn').addEventListener('click', function() {
        li.remove()
      })
      
      // Ajouter à la liste et effacer la saisie
      listeTaches.appendChild(li)
      saisie.value = ''
    })
  }
})
</script>

---

# Délégation d'Événements

Tirez parti du bouillonnement d'événements pour gérer les événements de plusieurs éléments avec un seul écouteur.

```html
<ul id="task-list">
  <li>Tâche 1</li>
  <li>Tâche 2</li>
  <li>Tâche 3</li>
  <!-- Plus d'éléments peuvent être ajoutés dynamiquement -->
</ul>
```

```js
const listeTaches = document.getElementById('task-list');

// Un seul écouteur d'événements pour tous les éléments, même ceux ajoutés ultérieurement
listeTaches.addEventListener('click', function(evenement) {
  // Vérifier si l'élément cliqué est un li
  if (evenement.target.tagName === 'LI') {
    evenement.target.classList.toggle('completed');
  }
});
```

<div class="mt-4 bg-green-100 p-4 rounded-lg">
  <strong class="text-green-800">Avantages :</strong>
  <ul class="text-green-800">
    <li>Moins d'écouteurs d'événements = meilleures performances</li>
    <li>Fonctionne pour les éléments ajoutés dynamiquement</li>
    <li>Utilisation de mémoire réduite</li>
  </ul>
</div>

---

# Événements DOM Courants

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Événements de Souris</h3>
    <ul>
      <li><code>click</code> - Quand un élément est cliqué</li>
      <li><code>dblclick</code> - Quand un élément est double-cliqué</li>
      <li><code>mousedown/mouseup</code> - Appui/relâchement du bouton de la souris</li>
      <li><code>mouseover/mouseout</code> - Le curseur entre/quitte un élément</li>
      <li><code>mousemove</code> - Le curseur se déplace sur un élément</li>
    </ul>
  </div>
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Événements Clavier</h3>
    <ul>
      <li><code>keydown</code> - Quand une touche est enfoncée</li>
      <li><code>keyup</code> - Quand une touche est relâchée</li>
      <li><code>keypress</code> - Quand une touche est pressée (touches de caractères)</li>
    </ul>
    
    <h3 class="text-yellow-500 text-xl mb-2 mt-4">Événements de Formulaire</h3>
    <ul>
      <li><code>submit</code> - Quand un formulaire est soumis</li>
      <li><code>change</code> - Quand la valeur d'un contrôle de formulaire change</li>
      <li><code>input</code> - Quand la valeur d'un élément input change</li>
      <li><code>focus/blur</code> - Quand un élément obtient/perd le focus</li>
    </ul>
  </div>
</div>

---

# Démo Interactive : Types d'Événements

<div class="w-full max-w-lg mx-auto">
  <div id="event-sandbox" class="bg-white p-6 rounded-lg shadow-lg">
    <div class="mb-6">
      <h3 class="text-lg font-bold mb-2">Terrain de Jeu d'Événements</h3>
      <p class="text-sm">Interagissez avec l'élément ci-dessous pour voir différents événements en action</p>
    </div>
    
    <div id="event-target" class="w-full h-32 bg-blue-100 border-2 border-blue-300 rounded-lg flex items-center justify-center text-lg font-medium cursor-pointer">
      Interagissez avec moi !
    </div>
    
    <div class="mt-4">
      <h4 class="font-bold">Journal d'Événements :</h4>
      <div id="event-monitor" class="mt-2 h-32 overflow-y-auto bg-gray-100 p-2 rounded text-sm font-mono"></div>
    </div>
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const cible = document.getElementById('event-target')
  const moniteur = document.getElementById('event-monitor')
  
  if (cible && moniteur) {
    const typesEvenements = ['click', 'dblclick', 'mouseenter', 'mouseleave', 'mousedown', 'mouseup', 'contextmenu']
    
    function enregistrerEvenement(e) {
      const log = document.createElement('div')
      log.textContent = `${e.type} à (${e.clientX}, ${e.clientY})`
      moniteur.prepend(log)
    }
    
    typesEvenements.forEach(type => {
      cible.addEventListener(type, enregistrerEvenement)
    })
  }
})
</script>

```js
const cible = document.getElementById('event-target');
const moniteur = document.getElementById('event-monitor');

const typesEvenements = ['click', 'dblclick', 'mouseenter', 'mouseleave', 
                         'mousedown', 'mouseup', 'contextmenu'];

function enregistrerEvenement(e) {
  const log = document.createElement('div');
  log.textContent = `${e.type} à (${e.clientX}, ${e.clientY})`;
  moniteur.prepend(log);
}

typesEvenements.forEach(type => {
  cible.addEventListener(type, enregistrerEvenement);
});
```

---

# Bonnes Pratiques

<div class="grid grid-cols-2 gap-6">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Performance</h3>
    <ul>
      <li>Utilisez la délégation d'événements pour plusieurs éléments</li>
      <li>Évitez les manipulations DOM excessives</li>
      <li>Regroupez les mises à jour DOM quand c'est possible</li>
      <li>Envisagez d'utiliser `requestAnimationFrame` pour les animations</li>
      <li>Évitez le "layout thrashing" (alternance de lectures et écritures)</li>
    </ul>
    
    <h3 class="text-yellow-500 text-xl mb-2 mt-4">Organisation du Code</h3>
    <ul>
      <li>Séparez la logique de la manipulation DOM</li>
      <li>Utilisez des noms de variables descriptifs</li>
      <li>Commentez les opérations DOM complexes</li>
    </ul>
  </div>
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Gestion des Erreurs</h3>
    <ul>
      <li>Vérifiez toujours si les éléments existent avant de les manipuler</li>
      <li>Utilisez try/catch pour les opérations DOM risquées</li>
      <li>Validez les saisies utilisateur avant de les traiter</li>
    </ul>
    
    <h3 class="text-yellow-500 text-xl mb-2 mt-4">Accessibilité</h3>
    <ul>
      <li>Utilisez des éléments HTML sémantiques</li>
      <li>Assurez-vous que la navigation au clavier fonctionne</li>
      <li>Maintenez la gestion du focus</li>
      <li>Ajoutez les attributs ARIA appropriés</li>
      <li>Testez avec des lecteurs d'écran</li>
    </ul>
  </div>
</div>

---

# Construisons Quelque Chose Ensemble

<div class="w-full max-w-lg mx-auto bg-white p-6 rounded-lg shadow-lg">
  <h3 class="text-xl font-bold mb-4">Galerie d'Images Interactive</h3>
  
  <div id="gallery-container" class="mb-4">
    <div id="main-image" class="w-full h-64 bg-gray-200 rounded-lg flex items-center justify-center mb-4">
      <span class="text-gray-500">Sélectionnez une vignette</span>
    </div>
    
    <div id="thumbnails" class="grid grid-cols-4 gap-2">
      <div class="thumbnail h-16 bg-blue-300 rounded cursor-pointer" data-color="blue"></div>
      <div class="thumbnail h-16 bg-red-300 rounded cursor-pointer" data-color="red"></div>
      <div class="thumbnail h-16 bg-green-300 rounded cursor-pointer" data-color="green"></div>
      <div class="thumbnail h-16 bg-yellow-300 rounded cursor-pointer" data-color="yellow"></div>
    </div>
  </div>
  
  <div class="text-sm">
    Ensemble, nous allons implémenter la fonctionnalité de galerie en direct !
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const imageprincipale = document.getElementById('main-image')
  const vignettes = document.getElementById('thumbnails')
  
  if (imageprincipale && vignettes) {
    vignettes.addEventListener('click', function(event) {
      if (event.target.classList.contains('thumbnail')) {
        const couleur = event.target.dataset.color
        imageprincipale.style.backgroundColor = `${couleur}`
        
        let texte = 'BLEU'
        if (couleur === 'red') texte = 'ROUGE'
        if (couleur === 'green') texte = 'VERT'
        if (couleur === 'yellow') texte = 'JAUNE'
        
        imageprincipale.innerHTML = `<span class="text-white font-bold">${texte} sélectionné</span>`
      }
    })
  }
})
</script>

```js
// Exemple de code pour la galerie
const imageprincipale = document.getElementById('main-image');
const vignettes = document.getElementById('thumbnails');

// Utiliser la délégation d'événements pour toutes les vignettes
vignettes.addEventListener('click', function(event) {
  if (event.target.classList.contains('thumbnail')) {
    const couleur = event.target.dataset.color;
    
    // Mettre à jour l'image principale
    imageprincipale.style.backgroundColor = `${couleur}`;
    
    // Traduire les couleurs en français
    let texte = 'BLEU';
    if (couleur === 'red') texte = 'ROUGE';
    if (couleur === 'green') texte = 'VERT'; 
    if (couleur === 'yellow') texte = 'JAUNE';
    
    imageprincipale.innerHTML = `<span class="text-white font-bold">
      ${texte} sélectionné
    </span>`;
  }
});
```

---
class: text-center
---

# Points Clés à Retenir

<div class="grid grid-cols-2 gap-8">
  <div class="text-left">
    <h3 class="text-yellow-500 text-xl mb-4">Maîtrise du DOM</h3>
    <ul>
      <li>Comprendre la structure du DOM</li>
      <li>Sélectionner et manipuler des éléments</li>
      <li>Créer et supprimer des éléments</li>
      <li>Gérer les attributs et styles des éléments</li>
    </ul>
  </div>
  <div class="text-left">
    <h3 class="text-yellow-500 text-xl mb-4">Maîtrise des Événements</h3>
    <ul>
      <li>Utiliser addEventListener</li>
      <li>Comprendre la propagation des événements</li>
      <li>Implémenter la délégation d'événements</li>
      <li>Travailler avec divers types d'événements</li>
      <li>Gérer correctement les interactions utilisateur</li>
    </ul>
  </div>
</div>

<div class="mt-8">
  <p>Le DOM et les événements sont fondamentaux pour créer des applications web interactives !</p>
</div>

---
layout: end
---

# Merci !

<div class="text-xl">
  Des Questions ? Discutons-en !
</div>