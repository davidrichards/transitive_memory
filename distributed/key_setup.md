# Setting up Keys

Check local keys:

```
ls ~/.ssh/id_rsa ~/.ssh/id_rsa.pub
```

Generate rsa key pair:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Copy a public key to the clipboard:

```
cat ~/.ssh/id_rsa.pub | pbcopy
```

Test a connection:

```
ssh -T git@github.com
```

Mounting a container with keys:

```
docker run -it --rm \
  -v ~/.ssh:/root/.ssh:ro \
  -v $(pwd):/app \
  -w /app \
  your-image-name
```

Copying keys into Dockerfile:

```
# Copy SSH keys into the container
COPY ~/.ssh /root/.ssh

# Set proper permissions
RUN chmod 600 /root/.ssh/id_rsa && \
    chmod 644 /root/.ssh/id_rsa.pub && \
    chmod 700 /root/.ssh
```

Run a container with port forwarding:

```
docker run -it --rm \
  -v $SSH_AUTH_SOCK:/ssh-agent \
  -e SSH_AUTH_SOCK=/ssh-agent \
  -v $(pwd):/app \
  -w /app \
  your-image-name
```

Test if the agent is running on the host:

```
eval $(ssh-agent)
ssh-add ~/.ssh/id_rsa
```

Check the git configuration:

```git config --global user.name "David Richards"
git config --global user.email "davidlamontrichards@gmail.com"
```

Prevent SSH warngings with a known_hosts file:

```
RUN mkdir -p /root/.ssh && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts
```

