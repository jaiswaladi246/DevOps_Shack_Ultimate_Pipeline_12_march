
## Installing Jenkins on Debian-based Systems

### 1. Install OpenJDK 17 Runtime Environment

```bash
sudo apt install openjdk-17-jre-headless -y
```

### 2. Add Jenkins Repository Key

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

### 3. Add Jenkins Repository to APT Sources

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### 4. Update APT Package Index

```bash
sudo apt-get update
```

### 5. Install Jenkins

```bash
sudo apt-get install jenkins -y
```

### 6. Start Jenkins Service

```bash
sudo systemctl start jenkins
```

### 7. Enable Jenkins Service to Start on Boot

```bash
sudo systemctl enable jenkins
```

### 8. Access Jenkins Web Interface

Once Jenkins is installed and running, you can access its web interface using a web browser. Navigate to `http://<server-ip-or-domain>:8080` to access the Jenkins dashboard.

