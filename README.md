# asterisk

Configuration d'exemple d'un serveur de téléphonie **Asterisk** utilisée en formation : deux postes SIP, une messagerie vocale et deux serveurs vocaux interactifs (SVI) avec synthèse vocale en français.

## Contenu

| Fichier | Rôle |
|---|---|
| `users.conf` | Déclaration des comptes SIP/IAX : `6001` (John DOE) et `6002` (Albert DUPONT), contexte `work` |
| `extensions.conf` | Plan de numérotation (dialplan) : appels internes, messagerie vocale, SVI |
| `voicemail.conf` | Boîtes vocales des postes 6001 et 6002 |

## Plan de numérotation

| Numéro | Action |
|---|---|
| `6XXX` | Appel du poste SIP correspondant (20 s), puis messagerie vocale si pas de réponse |
| `600` | Consultation de sa messagerie vocale |
| `8000` | SVI n°1 : message d'accueil puis raccroché |
| `8001` | SVI n°2 : menu vocal — touche `1` → John Doe, touche `2` → Albert Dupont, `#` → réécouter |

Les messages des SVI sont générés à la volée par le script AGI [`googletts.agi`](https://github.com/zaf/asterisk-googletts) (synthèse vocale Google, langue `fr`).

## Mise en place

```bash
sudo apt install asterisk
sudo cp users.conf extensions.conf voicemail.conf /etc/asterisk/
# googletts.agi doit être présent dans /var/lib/asterisk/agi-bin/
sudo asterisk -rx "core reload"
```

Configurez ensuite deux softphones (Zoiper, Linphone…) avec les identifiants `6001` / `6002`.

## ⚠️ Sécurité

Les mots de passe SIP (`123456`) et les codes de messagerie (`1234`) sont volontairement simplistes pour le TP. **Ne pas exposer cette configuration sur Internet** : un serveur SIP avec des secrets faibles est très vite exploité (fraude téléphonique).
