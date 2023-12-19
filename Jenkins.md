## Jenkins

### Installation

- download jenkins.war

- go to the folder where jenkins.war is

- java -jar jenkins.war

- save the password 

  - 54de71e276f24bf8b6f3f2c05d4b7ca5

    This may also be found at: `C:\Users\Voltage\.jenkins\secrets\initialAdminPassword`

- go to the browser http://localhost:8080 (也可用本机ip, http://192.168.1.114:8080)

### Install Jenkins on Ubuntu20.04

```sh
sudo apt update
sudo apt install openjdk-11-jdk
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
sudo ufw allow 8080
sudo ufw enable
```



### Plugin

- Role Strategy -- manage user roles
  - Manage Jenkins -> security -> Authorization -> Role-Based Strategy
- Git
- Publish over SSH

### Configuration

System -> Publish Over SSH
