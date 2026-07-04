# docker-netbird
Netbird


Auto script is not ready. Basically run the getting-started.sh to generate the database. Copy the files from the named database to the LOCVOL1/netbird folder, update items:

config.yaml:
- exposedAddress
- authSecret
- issuer
- dashboardRedirectURIs
- encryptionKey

dashboard.yaml (all 3 of these should be your https domain)
- NETBIRD_MGMT_API_ENDPOINT
- NETBIRD_MGMT_GRPC_API_ENDPOINT
- AUTH_AUTHORITY

```bash
export MYDOMAINIS=https://netbird.example.io
sed -i 's;YourDomainHere;$MYDOMAINIS$;g' dashboard.env
```



## Install
Run the quick setup script to generate the files needed, which will also generate the encryption keys.

Run the getting-started script (in a temp directory), follow the prompts:
```bash
rm -fr /opt/stacks/netbird
mkdir -p /opt/stacks/netbird
mkdir -p /tmp/netbirdsetup
cd /tmp/netbirdsetup
curl -fsSL https://github.com/netbirdio/netbird/releases/latest/download/getting-started.sh | bash
docker compose down
cp config.yaml dashboard.env docker-compose.yml /opt/stacks/netbird
#convert named docker volume to accessible mount
docker run --rm -v netbirdsetup_netbird_data:/source:ro -v /tmp/netbirdsetup:/backup alpine tar czf /backup/volume_backup.tar.gz -C /source .
docker run --rm -v /opt/docker/netbird:/target -v /tmp/netbirdsetup:/backup alpine tar xzf /backup/volume_backup.tar.gz -C /target
```










```bash
rm -fr /opt/stacks/netbird
mkdir -p /opt/stacks/netbird/generator
cd /opt/stacks/netbird/generator
export NETBIRD_DOMAIN=netbird.example.com; curl -fsSL https://github.com/netbirdio/netbird/releases/latest/download/getting-started.sh | bash
```

After running through the auto config, lets copy the files to a new stacks location (up 1 level):

```bash
cd /opt/stacks/netbird/generator
docker compose down --volumes
cp config.yaml dashboard.env docker-compose.yml ..
cd /opt/stacks/netbird
mv docker-compose.yml compose.yaml
sed -i 's;netbird_data:/;{$LOCVOL1}netbird:/;g' compose.yaml
```










**Clone git to system**
```bash
rm -fr /opt/stacks/hawser
mkdir -p /opt/stacks/hawser
curl -fsSL https://raw.githubusercontent.com/FinchTechSoCal/docker-stacks/refs/heads/main/hawser/.env -o /opt/stacks/hawser/.env
curl -fsSL https://raw.githubusercontent.com/FinchTechSoCal/docker-stacks/refs/heads/main/hawser/compose.yml -o /opt/stacks/hawser/compose.yml
sed -i 's;AGENT_NAME=;AGENT_NAME='$(cat /etc/hostname)';g' /opt/stacks/hawser/.env
```



# docker-stacks
Docker stacks

Current (new) docker stacks location

## Host setup

**Install docker**

See: https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh
```
