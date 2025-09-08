# LDAP-NGIX-CLOUD

This compose file deploys an Active Directory domain controller using Samba, an NGINX reverse proxy, an LDAP service and a RADIUS server. The services share a bridged network (`192.168.65.0/24`) so they can be reached by other networks such as `192.168.1.0/24` and `192.168.13.0/27` via your router.

## Services

- **TEAMELITEAD** – Samba Active Directory domain controller for the domain `SAMBA-DOMAIN`.
- **NGINX** – Reverse proxy exposing HTTP and HTTPS on the container host. Forward these ports on your router to the container's IP within `192.168.65.0/24`.
- **OpenLDAP** – Provides LDAP directory service that can be used by other devices (e.g. UniFi Dream Machine) for RADIUS authentication.
- **FreeRADIUS** – Example RADIUS server pointed at the LDAP service. Further configuration is required before production use.

## Network

The `adnet` network is created with a static subnet of `192.168.65.0/24`. Each service is assigned a static IP so that routing and firewall rules can be applied consistently. Ensure your router has routes allowing `192.168.1.0/24` and `192.168.13.0/27` to reach this subnet.

## Volumes

The Active Directory service stores its database on the `ad-data` volume, which is limited to 15GB.

## Usage

1. Adjust passwords and domain names in `docker-compose.yml` as needed.
2. Start the stack:
   ```
   docker compose up -d
   ```
3. Configure your router to forward ports 80 and 443 to the IP assigned to the NGINX container (e.g. `192.168.65.20`).
4. Configure your Dream Machine to use the LDAP or RADIUS services as required.
