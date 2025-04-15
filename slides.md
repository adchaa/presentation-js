---
theme: geist
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

# Ce Que Nous Allons Couvrir (TODO : LAZMNI NBADIL FI PAGE HATHI)

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

# HTML DOM

Lorsqu'une page web est chargée, le navigateur crée DOM (Document Object Model) de la page

<div class="flex justify-center items-center">
```mermaid
graph TD
    A[Document] --> B[html]
    B --> C[head]
    B --> D[body]
    C --> E[title]
    D --> F[h1]
    D --> G[div]
```
</div>

---

## JavaScript obtient toute la puissance nécessaire pour créer du HTML dynamique:

<ul class="list-disc pl-6 space-y-2 text-lg">
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut modifier tous les éléments HTML de la page</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut modifier tous les attributs HTML de la page</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut modifier tous les styles CSS de la page</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut supprimer des éléments et des attributs HTML existants</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut ajouter de nouveaux éléments et attributs HTML</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut réagir à tous les événements HTML existants dans la page</li>
  <li class="text-gray-200 hover:text-blue-600 transition-colors">JavaScript peut créer de nouveaux événements HTML dans la page</li>
</ul>

---

# Trouver des Éléments HTML

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Méthodes de Sélection</h3>
    
```js
// Par ID - retourne un élément unique
const element = document.getElementById('monId');

// Par nom de classe - retourne une HTMLCollection
const elements = document.getElementsByClassName('maClasse');

// Par nom de balise - retourne une HTMLCollection
const paragraphes = document.getElementsByTagName('p');

// Par nom - retourne une NodeList (formulaires)
const inputs = document.getElementsByName('email');
```
  </div>
  
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Sélecteurs CSS (Modernes)</h3>
    
```js
// Sélectionne le premier élément correspondant
const premier = document.querySelector('.classe');

// Sélectionne tous les éléments correspondants
// Retourne une NodeList
const tous = document.querySelectorAll('p.important');

// Sélecteurs complexes
const elements = document.querySelectorAll(
  'div.container > p:first-child'
);

// Vérifier l'existence d'un élément
if (document.querySelector('.elementSpecial')) {
  // L'élément existe
}
```
  </div>
</div>

---

# Collections HTML et NodeList

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">HTMLCollection</h3>
    
```js
// Retourne une HTMLCollection
const divs = document.getElementsByTagName('div');

// Accès par index
const premierDiv = divs[0];

// Accès par ID
const divSpecial = divs.namedItem('divId');

// ⚠️ Collection DYNAMIQUE (mise à jour automatique)
// quand le DOM change
console.log(divs.length); // 5
document.body.appendChild(document.createElement('div'));
console.log(divs.length); // 6 (mise à jour automatique)

// Conversion en tableau pour utiliser les méthodes Array
const tabDivs = Array.from(divs);
tabDivs.forEach(div => div.style.color = 'blue');
```
  </div>
  
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">NodeList</h3>
    
```js
// Retourne une NodeList
const paras = document.querySelectorAll('p');

// Accès par index
const premierPara = paras[0];

// ⚠️ Collection STATIQUE (pas de mise à jour)
console.log(paras.length); // 3
document.body.appendChild(document.createElement('p'));
console.log(paras.length); // Toujours 3 (ne change pas)

// Possède forEach natif (mais pas toutes les méthodes Array)
paras.forEach(para => para.classList.add('texte'));

// Conversion complète en tableau si nécessaire
const tabParas = Array.from(paras);
const filtre = tabParas.filter(p => p.textContent.includes('mot'));
```
  </div>
</div>

---

# Modifier des Éléments HTML

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Contenu et Attributs</h3>
    
```js
// Modifier le contenu HTML
element.innerHTML = '<span>Nouveau contenu</span>';

// Modifier le contenu texte (plus sécurisé)
element.textContent = 'Texte simple';

// Modifier des attributs (méthode 1)
element.setAttribute('class', 'active');
element.setAttribute('href', 'https://exemple.com');

// Modifier des attributs (méthode 2)
element.className = 'active';
element.href = 'https://exemple.com';
```
```html
<a class="active" href="https://exemple.com"></a>
```
  </div>
  
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Styles et Classes</h3>
    
```js
// Modifier les styles directement
element.style.color = 'red';
element.style.backgroundColor = '#f0f0f0';
element.style.fontSize = '18px';

// Ajouter/supprimer des classes
element.classList.add('actif');
element.classList.remove('inactif');
element.classList.toggle('selectionne');
element.classList.replace('ancien', 'nouveau');

// Vérifier si une classe existe
if (element.classList.contains('important')) {
  // Faire quelque chose
}
```
  </div>
</div>

---

# Ajouter et Supprimer des Éléments

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Créer et Ajouter</h3>
    
```js
// Créer un nouvel élément
const div = document.createElement('div');
const para = document.createElement('p');

// Ajouter à la fin d'un élément parent
document.body.appendChild(div);
parentElement.appendChild(para);

// Insérer avant un élément spécifique
parentElement.insertBefore(nouvelElement, referenceElement);

// Méthodes modernes (meilleur support)
parent.append(element1, element2); // Ajoute à la fin
parent.prepend(element); // Ajoute au début
reference.before(element); // Insère avant
reference.after(element); // Insère après
```
  </div>
  
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Supprimer et Remplacer</h3>
    
```js
// Supprimer un élément (méthode moderne)
element.remove();

// Supprimer un élément enfant
parent.removeChild(element);

// Remplacer un élément
parent.replaceChild(nouvelElement, ancienElement);

// Méthode moderne pour remplacer
ancienElement.replaceWith(nouvelElement);

// Écrire directement dans le flux HTML (DÉCONSEILLÉ)
// Efface tout le contenu existant !
document.write('<p>Nouveau contenu</p>');
```
</div>
</div>

---

# De la Manipulation du DOM aux Événements

<div class="flex flex-col items-center justify-center h-80">
  <div class="text-5xl font-bold mb-8 bg-gradient-to-r from-blue-400 to-purple-500 text-transparent bg-clip-text">
    Structure ⟹ Interaction
  </div>
  <div class="grid grid-cols-2 gap-8">
    <div class="flex flex-col items-center">
      <div class="text-6xl mb-4">🏗️</div>
      <div class="text-center">
        <div class="text-xl font-semibold text-blue-400">Manipulation du DOM</div>
        <div class="text-sm text-gray-400 mt-2">Création et modification d'éléments</div>
      </div>
    </div>
    <div class="flex flex-col items-center">
      <div class="text-6xl mb-4">🖱️</div>
      <div class="text-center">
        <div class="text-xl font-semibold text-purple-500">Événements</div>
        <div class="text-sm text-gray-400 mt-2">Interactivité et réponse aux actions</div>
      </div>
    </div>
  </div>
</div>

---

# Événements HTML Courants

<div class="grid grid-cols-3 gap-4">
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Événements Souris</h3>
    <ul class="space-y-1 text-sm">
      <li><code>onclick</code> - Clic sur l'élément</li>
      <li><code>ondblclick</code> - Double clic</li>
      <li><code>onmouseover</code> - Souris entre dans l'élément</li>
      <li><code>onmouseout</code> - Souris quitte l'élément</li>
      <li><code>onmousedown</code> - Bouton souris enfoncé</li>
      <li><code>onmouseup</code> - Bouton souris relâché</li>
      <li><code>onmousemove</code> - Souris déplacée sur élément</li>
      <li><code>oncontextmenu</code> - Clic droit (menu contextuel)</li>
    </ul>
  </div>
  
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Formulaires & Clavier</h3>
    <ul class="space-y-1 text-sm">
      <li><code>onsubmit</code> - Formulaire soumis</li>
      <li><code>onreset</code> - Formulaire réinitialisé</li>
      <li><code>onfocus</code> - Élément reçoit le focus</li>
      <li><code>onblur</code> - Élément perd le focus</li>
      <li><code>onchange</code> - Valeur du champ changée</li>
      <li><code>oninput</code> - Saisie dans champ de texte</li>
      <li><code>onkeydown</code> - Touche enfoncée</li>
      <li><code>onkeyup</code> - Touche relâchée</li>
      <li><code>onkeypress</code> - Touche pressée (caractère)</li>
      <li><code>onselect</code> - Texte sélectionné</li>
    </ul>
  </div>
  
  <div>
  <div>
    <h3 class="text-yellow-500 text-xl mb-2">Document & Ressources</h3>
    <ul class="space-y-1 text-sm">
      <li><code>onload</code> - Élément chargé</li>
      <li><code>onunload</code> - Page déchargée</li>
      <li><code>onresize</code> - Fenêtre redimensionnée</li>
      <li><code>onscroll</code> - Défilement de la page</li>
      <li><code>onerror</code> - Erreur de chargement</li>
      <li><code>onabort</code> - Chargement interrompu</li>
      <li><code>onbeforeunload</code> - Avant déchargement</li>
    </ul>
    
  </div>
  <div>
    <h3 class="text-yellow-500 text-xl mb-2 mt-3">Tactile</h3>
    <ul class="space-y-1 text-sm">
      <li><code>ontouchstart</code> - Début toucher</li>
      <li><code>ontouchend</code> - Fin toucher</li>
      <li><code>ontouchmove</code> - Mouvement tactile</li>
    </ul>
  </div>
  </div>
</div>

<div class="mt-4 p-3 rounded-lg text-center text-sm">
  <span class="font-bold">Note:</span> Tous ces événements peuvent être utilisés comme attributs HTML (onclick="...") 
  <br>ou attachés via JavaScript avec addEventListener('click', ...)
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

# Mouse Hover

<div class="grid grid-cols-2 gap-8 h-full">
  <div class="flex flex-col justify-center pl-20 pt-10">
    <div id="mouse" class="flex justify-center items-center border-2 border-blue-400 rounded-lg w-52 h-52 text-center bg-gray-800 text-blue-300 font-mono text-xl shadow-lg transition-all duration-300">
      Déplacez le curseur ici
    </div>
  </div>
  <div class="flex flex-col justify-center pl-4">
    <h3 class="text-yellow-500 text-xl mb-4">Code</h3>
    
  ```js
  const mouse = document.getElementById("mouse");

  mouse.addEventListener("mousemove", (e) => {
    // Récupérer les coordonnées de la souris
    const x = e.clientX;
    const y = e.clientY;
    
    mouse.textContent = `(${x}, ${y})`;
  });
  ```
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const mouse = document.getElementById("mouse")
  mouse.addEventListener("mousemove",(e)=>{
    mouse.textContent = `(${e.clientX}, ${e.clientY})`
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

# Démo : Événements Clavier

<div class="grid grid-cols-2 gap-8 h-full">
  <div class="flex flex-col justify-center items-center mt-9">
    <input id="keyboard-input" class="w-64 p-3 bg-gray-900 border-2 border-blue-400 rounded-lg text-center text-blue-300 font-mono text-xl focus:outline-none focus:border-yellow-400" type="text" placeholder="Cliquez ici d'abord">
    <div id="key-display" class="mt-6 p-4 w-64 h-24 bg-gray-900 border-2 border-green-400 rounded-lg flex items-center justify-center text-green-300 font-mono text-xl">
      Appuyez sur une touche
    </div>
  </div>
  <div class="flex flex-col justify-center pl-4">
    <h3 class="text-yellow-500 text-xl mb-4">Code</h3>
    
```js
const input = document.getElementById("keyboard-input")
const display = document.getElementById("key-display")

if (input && display) {
  input.addEventListener("keydown", (e) => {
    display.textContent = `Touche: ${e.key}\nCode: ${e.code}`
    
    if (e.key === "h") {
      display.textContent = "special"
    }
  })
}
```
  </div>
</div>

<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const input = document.getElementById("keyboard-input")
  const display = document.getElementById("key-display")
  
  if (input && display) {
    input.addEventListener("keydown", (e) => {
      display.textContent = `Touche: ${e.key}\nCode: ${e.code}`
      
      if (e.key === "h") {
        display.textContent = "special"
      }
    })
  }
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
    <div class="text-center relative bottom-4">Document</div>
    <div class="absolute top-12 left-12 right-12 bottom-12 border-4 border-green-500 rounded-lg p-4">
      <div class="text-center">Parent</div>
      <div class="absolute top-12 left-8 right-8 bottom-8 border-4 border-red-500 rounded-lg flex items-center justify-center">
        <div class="text-center">Cible</div>
      </div>
    </div>
  </div>
</div>

---

# Démo Interactive : Propagation d'Événements

<div class="grid grid-cols-2 gap-8">
  <div>
    <div id="parent" class="border-2 border-blue-200 p-8 rounded-lg relative">
      <div class="text-center mb-4">Élément Parent</div>
      <div id="enfant" class="border-2 border-green-200 p-6 rounded-lg">
        <div class="text-center mb-2">Élément Enfant</div>
        <button id="btn" class="border-2 border-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-500">Cliquez-moi</button>
      </div>
    </div>
    <div id="event-log" class="mt-4 h-32 overflow-y-auto border-2 border-gray-100 p-2 rounded text-white"></div>
  </div>
  <div>
```js
const parent = document.getElementById('parent');
const enfant = document.getElementById('enfant');
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
  const enfant = document.getElementById('enfant')
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

# Techniques Avancées d'Événements

<div class="grid grid-cols-2 gap-4">
  <div>
    <h3 class="text-xl text-yellow-500 mb-2">Délégation d'Événements</h3>
```js
document.getElementById('container').addEventListener('click', function(e) {
  if (e.target.matches('button.action')) {
    console.log('Bouton cliqué:', e.target.textContent);
  }
});
```
    <div class="mt-2 text-green-500">✓ Parfait pour les éléments dynamiques</div>
    <h3 class="text-xl text-yellow-500 mb-4 mt-4">Supprimer des Écouteurs</h3>

```js
function handleClick(e) {
  e.currentTarget.removeEventListener('click', handleClick);
}

element.addEventListener('click', handleClick);
```
  </div>
  
  <div>
    <h3 class="text-xl text-yellow-500 mb-2">Options de addEventListener</h3>

```js
element.addEventListener('click', handler, {
  once: true,      // Se déclenche une seule fois puis se supprime
  capture: true,   // Capture pendant la phase descendante
  passive: true    // Indique que preventDefault() ne sera pas appelé
});

element.addEventListener('click', function(e) {
  e.stopPropagation();
});

form.addEventListener('submit', function(e) {
  e.preventDefault(); // Empêche l'envoi du formulaire
});
```
    <div class="mt-2 text-blue-500">ℹ️ L'option 'passive' améliore les performances tactiles</div>
  </div>
</div>

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
layout: full
--- 

<div class="flex justify-center items-center w-full h-full">
  <div class="text-5xl font-bold mb-8 bg-gradient-to-r from-blue-400 to-purple-500 text-transparent bg-clip-text">
    Conway's Game of Life
  </div>
</div>

---

# Le Jeu de la Vie de Conway

<div>
  <h2 class="text-yellow-500 text-xl mb-2">Les Règles</h2>
  <ul className="space-y-6 text-xl">
    <li className="flex items-start">
      <span className="text-amber-400 text-xl mr-3">1.</span>
      <span>Une cellule vivante avec moins de 2 voisines vivantes meurt (sous-population)</span>
    </li>
    <li className="flex items-start">
      <span className="text-amber-400 text-xl mr-3">2.</span>
      <span>Une cellule vivante avec 2 ou 3 voisines vivantes survit</span>
    </li>
    <li className="flex items-start">
      <span className="text-amber-400 text-xl mr-3">3.</span>
      <span>Une cellule vivante avec plus de 3 voisines vivantes meurt (surpopulation)</span>
    </li>
    <li className="flex items-start">
      <span className="text-amber-400 mr-3">4.</span>
      <span>Une cellule morte avec exactement 3 voisines vivantes devient vivante (reproduction)</span>
    </li>
  </ul>
</div>