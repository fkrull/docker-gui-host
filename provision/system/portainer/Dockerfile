FROM portainer/portainer:latest
COPY endpoints.json /etc/
CMD ["--no-auth", "--external-endpoints=/etc/endpoints.json"]
