
## Configuration Details

- **Accounts Created**:
  - `marketing`: Configured with SSH key for secure access and permissions to upload files to the web server directory.
  - `design`: Configured with SSH key and shell access restricted to their home directory.
  - `audit`: Configured with SSH key and SFTP-only access to specified directories.
- **Deleted Account**:
  - `sysadmin`: Removed the account and its associated home directory.
- **Permissions**:
  - `marketing`: Write access to `/srv/www` (web server directory); no access to design or audit files.
  - `design`: Read/write access to their own home directory; no access to web server directory.
  - `audit`: Read-only access to marketing's home directory, design's home directory, and `/srv/www`.

### Task 2: File Transfer (SFTP)
- Configured SFTP with restricted shell (`/usr/lib/openssh/sftp-server`) for secure file transfers.
- **Access Controls**:
  - `marketing`: Read/write access to `/srv/www`.
  - `design`: Read/write access limited to their home directory.
  - `audit`: SFTP-only access with read-only permissions across relevant directories.

- Installed and configured **Nginx** as the HTTP server to serve static files.
- **Document Root**: `/srv/www`.
- Created the `/srv/www/student/` directory and a static file containing `661982`.
- Verified that the file is accessible at:
  - `http://[YOUR_VM_IP]/student/`
  - `http://student-661982-vm1.net.dcs.hull.ac.uk/student/`.
- **Nginx Configuration**:
  - Configured Nginx to serve the static file at `/student/`.
  - Set up a basic `server` block for the root domain (`student-700290-vm1.net.dcs.hull.ac.uk`).


- Installed Docker and cloned the repository from `https://github.com/sbrl/SDI-Docker.git`.
- Built and deployed the Docker container.
- Configured Docker to start on boot.
- Set up Nginx as a reverse proxy:
  - Default domain (`student-661982-vm1.net.dcs.hull.ac.uk`): Displays static content.
  - Subdomain (`docker.student-661982-vm1.net.dcs.hull.ac.uk`): Redirects to the Docker application running on port 3000.

## Maintenance Instructions
1. **Account and Password Management**:
   - To update a password: `sudo passwd [username]`.
   - To add a user: `sudo useradd [username]`.
   - To remove a user and their files: `sudo userdel -r [username]`.
2. **Web Server Management**:
   - Restart Nginx: `sudo systemctl restart nginx`.
   - Update web content: Add files to `/srv/www/`.
   - View logs: `sudo tail -f /var/log/nginx/access.log`.
3. **Docker Management**:
   - View running containers: `docker ps`.
   - Restart a container: `docker restart [container_id]`.
   - Pull updated images: `docker pull [repository/image]`.
4. **SFTP Monitoring**:
   - Monitor SFTP activity using `journalctl -u sshd`.
   - Update permissions if required with `chmod` and `chown`.

## Notes
- SSH keys are stored in `~/.ssh/authorized_keys` for each user.
- The `/srv/www` directory is owned by the `www-data` group, and only `marketing` has write permissions.
- All Docker configurations are stored in `/etc/docker/` for easy access.
- The firewall (`ufw`) is configured to allow only ports 22, 80, and 3000.
