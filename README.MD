# PODMAN : Manage Container 

## LAUNCH

### Build the image:
```bash
podman build -t my-portfolio:latest .
```

### List Images
```bash
podman images
```

### Launch Images
```bash
podman run -d --name portfolio-site -p 8080:80 localhost/my-portofolio:latest
```

8080 (The Host Port)
What it is: The port on your physical Linux machine.

Why we use it: By default, standard web traffic runs on port 80. However, Linux prevents normal users (rootless processes) from binding to any port below 1024 for security reasons. By choosing 8080, Podman can run entirely without sudo privileges.

80 (The Container Port)
What it is: The port inside the container where the application is listening.

Why we use it: Inside your container, the web server (Nginx) is configured to listen on standard HTTP port 80. It doesn't know (or care) what is happening on your host machine.

### CHECK LOCALLY
```
http://localhost:8080
```



## MANAGE CONTAINER

### View Active Containers
To see what is currently running on your system, run:
```bash
podman ps
```
This shows you the container ID, the image it is using, how long it has been up, and the port mappings.

Tip: To see all containers (both running and stopped), add the -a flag: podman ps -a.

### Check Live Web Traffic Logs
Every time someone visits your website or an asset loads, Nginx prints a log line. You can watch this in real time:

```bash
podman logs -f portfolio-site
```
(The -f stands for "follow", which keeps the terminal open and streams new hits as they happen. Press Ctrl+C to exit the log view).

### Stop and Start the Website
If you need to temporarily take the website offline (e.g., if you are stepping away or freeing up system memory):

```bash
podman stop portfolio-site
```

To bring it back online without rebuilding anything:

```bash
podman start portfolio-site
```

### Updating the Website Code
If you modify your HTML or CSS files in your ohansyah.github.io directory, the running container won't see the changes automatically because the files were baked into the image during Step 3.

To deploy updates, you simply chain these commands together:
```bash
# 1. Stop and remove the old container
podman stop portfolio-site
podman rm portfolio-site

# 2. Rebuild the image with your updated files
podman build -t my-portofolio:latest .

# 3. Launch the new version
podman run -d --name portfolio-site -p 8080:80 localhost/my-portofolio:latest
```


### Completely Clean Up
If you want to completely remove the container from your system to free up resources:
```bash
podman stop portfolio-site
podman rm portfolio-site
```