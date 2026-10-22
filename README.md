# nkezefuu-demo

Modules Odoo 20 de l'instance de démonstration **demo.nkezefuu.com**.

## Contenu

- Comptabilité Odoo Mates `om_account_accountant` 20.0.1.4.0 et ses dépendances (`om_*`, `accounting_pdf_reports`), licence LGPL-3.
- Les modules propres à la démonstration se rangent à la racine du dépôt, à côté des précédents : chaque dossier contenant un `__manifest__.py` est un module.

## Serveur

| Élément | Emplacement |
|---|---|
| Sources Odoo 20 et venv (partagés avec auchamp) | `/opt/nkezefuu/odoo20/odoo`, `/opt/nkezefuu/odoo20/venv` |
| Clone de ce dépôt | `/opt/nkezefuu/odoo20/demo/nkezefuu-demo` |
| Configuration | `/etc/nkezefuu-demo.conf` |
| Service systemd | `nkezefuu-demo` |
| Journal | `/var/log/nkezefuu_demo.log` |
| Base | `demo` |

## Déploiement

Un push sur `master` lance `.github/workflows/deploy.yml` : vérification des modules (Python, manifestes, XML, CSV, JSON), puis mise à jour du clone sur le serveur et redémarrage du service.

Le déploiement **ne met pas à jour les modules**. Après un changement de modèle, de vue, de droits ou de données, lancer à la main :

```bash
sudo systemctl stop nkezefuu-demo
sudo -u odoo /opt/nkezefuu/odoo20/venv/bin/python3 /opt/nkezefuu/odoo20/odoo/odoo-bin \
  -c /etc/nkezefuu-demo.conf -d demo -u <module> --stop-after-init
sudo systemctl start nkezefuu-demo
```

Secrets du dépôt : `VPS_HOST`, `VPS_USER`, `VPS_SSH_PRIVATE_KEY`.
