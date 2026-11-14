# Fréquences — relaxation sonore / sound relaxation

Petite application web personnelle qui joue des fréquences sonores (sinusoïdes) pour se détendre.
Sans serveur, sans compte, sans base de données. Fonctionne hors ligne une fois ouverte une première fois.

> Outil personnel de relaxation sonore. Ne remplace pas un avis médical.
> Statistiques anonymes seulement si vous acceptez, sans témoins.

**Adresse :** https://stefmagus.github.io/AAA_frequences/

---

## Français

### L'ouvrir sur un ordinateur
- Le plus simple : ouvrir l'adresse ci-dessus dans Chrome, Edge ou Firefox.
- En local (copie du dossier) : dans un terminal, aller dans le dossier `AAA_frequences`, taper
  `python3 -m http.server 8000`, puis ouvrir `http://localhost:8000` dans le navigateur.
  (Un double-clic sur `index.html` fonctionne aussi, mais sans le mode hors ligne.)

### L'installer sur un téléphone Android
1. Ouvrir l'adresse dans **Chrome**.
2. Toucher le bouton **📲 Installer l'application** (s'il apparaît), ou le menu **⋮** en haut à droite,
   puis **Ajouter à l'écran d'accueil** (ou **Installer l'application**).
3. Confirmer. Une icône « Fréquences » apparaît sur l'écran d'accueil ; elle s'ouvre comme une vraie application,
   même sans Internet.

### La partager avec des amis
Envoyez simplement l'adresse **https://stefmagus.github.io/AAA_frequences/** par texto, courriel ou messagerie.
Chaque personne garde ses propres favoris, programmes et historique sur son téléphone : rien n'est partagé.

### Bon à savoir
- Les programmes binauraux (🎧) demandent des écouteurs, un dans chaque oreille.
- Ne pas utiliser les programmes de sommeil ou de méditation en conduisant.
- Les sons aigus (plus de 5000 Hz) ont un volume limité à 30 %.
- Réglages → **Exporter mes données** pour garder une copie (fichier JSON), **Importer** pour la remettre.
- Statistiques : désactivées tant que vous n'avez pas répondu « Oui ». Seuls sont comptés l'ouverture de l'app,
  l'installation et le nom du programme lancé (jamais l'intention, l'historique ni vos programmes). Sans témoins.

---

## English

### Open it on a computer
- Easiest: open the address above in Chrome, Edge or Firefox.
- Locally (copy of the folder): in a terminal, go to the `AAA_frequences` folder, run
  `python3 -m http.server 8000`, then open `http://localhost:8000`.
  (Double-clicking `index.html` also works, but without offline mode.)

### Install it on an Android phone
1. Open the address in **Chrome**.
2. Tap **📲 Install the app** (if shown), or the **⋮** menu at the top right, then **Add to Home screen**
   (or **Install app**).
3. Confirm. A “Frequencies” icon appears on your home screen and opens like a regular app, even offline.

### Share it with friends
Just send the address **https://stefmagus.github.io/AAA_frequences/** by text, email or chat.
Everyone keeps their own favourites, programs and history on their own phone: nothing is shared.

---

## Fichiers / Files
| Fichier | Rôle |
|---|---|
| `index.html` | L'application (HTML + CSS + JS, CSP stricte) |
| `manifest.json` | Nom, icône, couleurs, mode « standalone » |
| `sw.js` | Service worker : met l'app en cache pour le hors-ligne (jamais GoatCounter) |
| `icon-192.png`, `icon-512.png` | Icône (onde sonore) |

### Note pour modifier `index.html`
La CSP autorise le script et le style intégrés par leur empreinte SHA-256. Après toute modification du
`<script>` ou du `<style>`, recalculer les empreintes, sinon la page ne fonctionnera plus :

```sh
python3 - <<'PY'
import re, hashlib, base64
s = open('index.html', encoding='utf-8').read()
h = lambda x: "'sha256-" + base64.b64encode(hashlib.sha256(x.encode()).digest()).decode() + "'"
sc = re.search(r'<script>(.*?)</script>', s, re.S).group(1)
st = re.search(r'<style>(.*?)</style>', s, re.S).group(1)
s = re.sub(r"(script-src 'self' https://gc\.zgo\.at )'sha256-[^']*'", lambda m: m.group(1) + h(sc), s)
s = re.sub(r"(style-src 'self' )'sha256-[^']*'", lambda m: m.group(1) + h(st), s)
open('index.html', 'w', encoding='utf-8').write(s)
PY
```
Puis changer `CACHE = 'frequences-v1'` dans `sw.js` (v2, v3…) pour que les téléphones reçoivent la mise à jour.
