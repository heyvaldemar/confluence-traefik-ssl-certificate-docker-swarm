# Confluence with SSL Certificate in a Docker Swarm

Configure Traefik and create secrets for storing the passwords on the Docker Swarm manager node before applying the configuration.

Traefik configuration: https://github.com/heyValdemar/traefik-ssl-certificate-docker-swarm

Create a secret for storing the password for Confluence database using the command:

`printf "YourPassword" | docker secret create confluence-postgres-password -`

Clear passwords from bash history using the command:

`history -c && history -w`

Run `confluence-restore-application-data.sh` on the Docker Swarm worker node where the container for backups is running to restore application data if needed.

Run `confluence-restore-database.sh` on the Docker Swarm node where the container for backups is running to restore database if needed.

Run `docker stack ps confluence | grep confluence_backups | awk 'NR > 0 {print $4}'` on the Docker Swarm manager node to find on which node container for backups is running.

Deploy Confluence in a Docker Swarm using the command:

`docker stack deploy -c confluence-traefik-ssl-certificate-docker-swarm.yml confluence`
