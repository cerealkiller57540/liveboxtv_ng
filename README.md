# Orange Livebox TV UHD — intégration Home Assistant

Intégration **Home Assistant** moderne pour les décodeurs **Orange Livebox TV UHD**
(modèle WHD94 / SoftAtHome et compatibles), pilotés via leur API locale `remoteControl`
et leur **MediaRenderer UPnP**.

Refonte complète (config flow, coordinator async, EPG dynamique, UPnP) de l'intégration
historique [AkA57/liveboxtvuhd](https://github.com/AkA57/liveboxtvuhd) — voir
[CREDITS](custom_components/liveboxtv_ng/CREDITS.md).

## ✨ Fonctionnalités

- 🔌 **Configuration par l'interface** (config flow) — plus de YAML.
- 🔎 **Découverte automatique** des décodeurs sur le réseau (**SSDP** *et* **DHCP**).
- 🧭 **Multi-décodeurs** : gère plusieurs box (salon, chambre…) sans conflit.
- 📡 **Liste des chaînes dynamique** depuis l'EPG officiel Orange (pas de liste figée
  dans le code → suit l'évolution du bouquet).
- 🎬 **EPG enrichi** : programme en cours (titre, image) sur la chaîne regardée.
- 🔊 **Volume / mute réels** via UPnP `RenderingControl` (MediaRenderer du décodeur).
- ▶️ **État de lecture** via UPnP `AVTransport`.
- 📺 **Zapping** (sélection de source), **télécommande** (entité `remote`, toutes les touches).
- 💤 **Robuste à la veille / coupure** : si le décodeur est éteint au démarrage de HA,
  l'entité devient simplement `unavailable` (jamais perdue) et revient seule au réveil.
- 🌐 **IP non réservée gérée** : l'identité repose sur l'adresse MAC ; un changement d'IP
  (DHCP) est suivi automatiquement via SSDP/DHCP.

## 📦 Installation

### Via HACS (dépôt personnalisé)
1. HACS → Intégrations → menu ⋮ → **Dépôts personnalisés**.
2. Ajouter `https://github.com/cerealkiller57540/liveboxtv_ng`, catégorie **Integration**.
3. Installer **Orange Livebox TV UHD**, puis **redémarrer** Home Assistant.

### Manuelle
Copier `custom_components/liveboxtv_ng/` dans le dossier `config/custom_components/` de
Home Assistant, puis redémarrer.

## ⚙️ Configuration

- En général, le décodeur est **découvert automatiquement** (SSDP/DHCP) : il apparaît dans
  **Paramètres → Appareils et services**, il suffit de confirmer.
- Sinon : **Ajouter une intégration → Orange Livebox TV UHD**, et saisir l'IP du décodeur.

> Prérequis : l'UPnP doit être actif sur la Livebox (Configuration avancée → Réseau → UPnP),
> ce qui est le cas par défaut.

## 🧩 Entités créées (par décodeur)

| Entité | Rôle |
|---|---|
| `media_player.<nom>` | État, chaîne, volume/mute, zapping, EPG en cours |
| `remote.<nom>_telecommande` | Envoi de touches (`send_command`) |

Touches disponibles pour `remote.send_command` : `POWER`, `0`–`9`, `CH+`/`CH-`,
`VOL+`/`VOL-`, `MUTE`, `UP`/`DOWN`/`LEFT`/`RIGHT`/`OK`, `BACK`, `MENU`, `PLAY/PAUSE`,
`FFWD`/`FBWD`, `REC`, `VOD`, `GUIDE`.

## ⚠️ Limites

- Le décodeur ne liste pas ses chaînes via l'API locale : la liste vient de l'EPG Orange
  (France). Hors France, un mapping de secours minimal est utilisé.
- L'API `remoteControl` n'expose pas de réglage de volume fin : le volume/mute « réels »
  passent par l'UPnP, le step volume passe par les touches télécommande.

## 📝 Licence

MIT — voir [LICENSE](LICENSE).

## 🙏 Crédits

Basé sur le travail de [AkA57](https://github.com/AkA57/liveboxtvuhd) et d'autres projets
de la communauté — détails dans [CREDITS](custom_components/liveboxtv_ng/CREDITS.md).
