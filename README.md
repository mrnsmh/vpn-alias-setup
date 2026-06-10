# VPN Alias Setup

Configuration et script pour tunneliser OpenCode, Hermes Agent, OpenClaw et `oc-go-cc` via un VPN Namecheap (FastVPN).

## Fonctionnalités

- **Script `vpn`** : Gestion du VPN via Docker (`dperson/openvpn-client`).
- **Bypass DNS** : Remplacement de `/etc/resolv.conf` dans le namespace réseau du conteneur.
- **Multi-outil** : Supporte `opencode`, `hermes`, `openclaw`, et `oc-go-cc`.
- **Switch rapide** : Changement de serveur VPN en une commande.

## Prérequis

- Docker
- Namecheap FastVPN (fichiers `.ovpn` et `credentials.txt`)

## Installation

1. Cloner ce dépôt :
   ```bash
   git clone https://github.com/<votre-user>/vpn-alias-setup.git
   cd vpn-alias-setup
   ```

2. Installer le script `vpn` :
   ```bash
   sudo cp vpn /usr/local/bin/vpn
   sudo chmod +x /usr/local/bin/vpn
   ```

3. Configurer le VPN :
   - Copiez vos fichiers `.ovpn` (TCP/UDP) dans `vpn-config/`.
   - Ajoutez vos identifiants dans `vpn-config/credentials.txt` :
     ```
     nom_utilisateur
     mot_de_passe
     ```

4. Démarrer le VPN :
   ```bash
   vpn change new-york
   ```

## Utilisation

| Commande | Description |
|----------|-------------|
| `vpn status` | Affiche l'état du VPN et l'IP publique |
| `vpn list` | Liste les serveurs disponibles |
| `vpn change <lieu>` | Change le serveur VPN |
| `vpn opencode [projet]` | Lance OpenCode sous VPN |
| `vpn hermes [projet]` | Lance Hermes Agent sous VPN |
| `vpn openclaw [projet]` | Lance OpenClaw sous VPN |
| `vpn oc-go-cc` | Lance `oc-go-cc` (proxy) sous VPN |
| `vpn stop` | Arrête le conteneur VPN |

## Structure

```
vpn-alias-setup/
├── vpn                    # Script principal
├── docker-compose.yml     # Configuration Docker (optionnel)
├── vpn-config/
│   ├── config.ovpn        # Config OpenVPN active
│   ├── credentials.txt    # Identifiants VPN
│   └── NCVPN-*.ovpn       # Fichiers de config serveur
└── README.md
```

## Notes

- Le script utilise `nsenter` pour exécuter les commandes dans le namespace réseau du conteneur Docker.
- Les DNS sont forcés sur `1.1.1.1` pour éviter les fuites DNS.
- Pour `oc-go-cc`, le proxy tourne sur le port `3456` (configurable).
