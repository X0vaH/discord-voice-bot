# 🔔 Discord Voice Bot — Notifications d'arrivée en vocal

Petit bot Discord en Python qui **détecte quand un membre rejoint un salon vocal** d'un serveur et **envoie un message privé** à l'utilisateur choisi (ex. : « Alex a rejoint **Général** »).

Pratique pour savoir quand des amis se connectent en vocal sans garder Discord ouvert en permanence.

## ✨ Fonctionnalités

- Message privé « Bot activé » au démarrage.
- Notification en message privé dès qu'un membre rejoint un salon vocal.
- Configuration par variables d'environnement (`.env`).
- Lancement automatique au démarrage de Windows (optionnel).

## 🧱 Stack technique

- Python 3.10+
- [discord.py](https://discordpy.readthedocs.io/)
- [python-dotenv](https://pypi.org/project/python-dotenv/)

---

## 🚀 Installation

### 1. Créer le bot sur Discord

1. Aller sur le [Discord Developer Portal](https://discord.com/developers/applications) → **New Application**.
2. Onglet **Bot** → **Reset Token** et copier le **token** (à garder secret !).
3. Toujours dans **Bot**, activer les *Privileged Gateway Intents* : **Server Members Intent**.
4. Onglet **OAuth2 → URL Generator** : cocher le scope `bot`, la permission `View Channels`, puis ouvrir l'URL générée pour inviter le bot sur votre serveur.

### 2. Récupérer les identifiants

1. Discord → **Paramètres → Avancés** → activer le **Mode développeur**.
2. Clic droit sur votre profil → **Copier l'identifiant de l'utilisateur** → `MY_USER_ID`.
3. Clic droit sur un salon du serveur → **Copier l'identifiant du salon** → `NOTIF_CHANNEL_ID`.

### 3. Installer le projet

```bash
git clone https://github.com/X0vaH/discord-voice-bot.git
cd discord-voice-bot

python -m venv .venv
# Windows :            .venv\Scripts\activate
# Linux / macOS / WSL : source .venv/bin/activate

pip install -r requirements.txt
```

### 4. Configurer

```bash
cp .env.example .env
```

Puis remplir `.env` :

```env
DISCORD_TOKEN=ton_token_de_bot
NOTIF_CHANNEL_ID=123456789012345678
MY_USER_ID=123456789012345678
```

> ⚠️ Ne jamais publier le fichier `.env` (il est ignoré par Git).

## ▶️ Lancement

```bash
python bot.py
```

La console affiche `Connecté en tant que <NomDuBot>.` et vous recevez « Bot activé » en message privé.

**Windows (via WSL)** : double-cliquer sur `start_bot.bat` (il lance `python bot.py` dans la distribution `Ubuntu-22.04` ; modifier le nom de la distribution dans le fichier si nécessaire).

### Lancer automatiquement au démarrage de Windows

1. `Win + R` → taper `shell:startup` → Entrée.
2. Placer un **raccourci** vers `start_bot.bat` dans ce dossier.

## 🛠️ Fonctionnement

```python
@client.event
async def on_voice_state_update(member, before, after):
    # before.channel est None et after.channel ne l'est pas => le membre vient de rejoindre
    if before.channel is None and after.channel is not None:
        await send_message(f"{member} a rejoint **{after.channel.name}**")
```

Le bot écoute l'événement `on_voice_state_update` et n'envoie une notification que lors d'une **arrivée** (pas lors d'un changement de salon ou d'un départ).

## 💡 Améliorations possibles

- Notifier aussi les départs et les changements de salon.
- Ignorer certains membres ou salons.
- Image Docker pour l'hébergement.

## 👤 Auteur

**Alex Héroux** ([@X0vaH](https://github.com/X0vaH))
