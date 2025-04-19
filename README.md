# 🚀 **Travel Memory Application**

🛠️ This repository mainly consists of:
- Travel Memory Application Deployment
- The Travel Memory application has been developed using the MERN stack. Your challenge is to deploy it on an Amazon EC2 instance.
- This will provide you with hands-on experience in deploying full-stack applications, working with cloud platforms, and ensuring scalable architecture.

---

## 📌 Table of Contents
- [Architecture Diagram of Application](#Architecture-Diagram-of-Application)
- [Backend Configuration](#Backend-Configuration)
- [Frontend and Backend Connection](#Frontend-and-Backend-Connection)
- [Scaling the Application](#Scaling-the-Application)
- [Domain Setup with Cloudflare](#Domain-Setup-with-Cloudflare)
- [🤝 Contributing](#contributing)
- [📜 License](#license)

---
**Architecture Diagram of Application**
![travelmemory agency drawio](https://github.com/user-attachments/assets/d7b98570-c22f-4e27-915f-a01b93e3d5e8)
---
**Task-1: Backend Configuration**
- Clone the repository and navigate to the backend directory.
  ```sh
  git clone https://github.com/UnpredictablePrashant/TravelMemory.git
  cd TravelMemory/backend
  ```
- Setup Node.js:
  ```sh
  sudo npm install
  ``` 
- Update the .env file to incorporate database connection details and port information.
  ```sh
  sudo nano .env
  MONGO_URI=your_mongodb_connection
  PORT=3000
  ```
- Start Backend (temporary start for checking)
  ```sh
  sudo npm start
  ```
 (Check if backend works on port 3000.)

- Setup Nginx Reverse Proxy for backend
  ```sh
  sudo nano /etc/nginx/sites-available/default
  ```
- Set up a reverse proxy using nginx to ensure smooth deployment on EC2. 
   ```sh
  server {
    listen 80;
    
    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
     }
  }
  ```
- Restart nginx
   ```sh
   sudo systemctl restart nginx
   ```
- To keep the backend running, permanently install pm2
   ```sh
    sudo npm install pm2 -g
    pm2 start index.js
    pm2 startup
    pm2 save
   ```
![image](https://github.com/user-attachments/assets/b4cfb8ed-c897-4ae8-9ab2-82416e1fa3fe)

**Task-2: Frontend and Backend Connection**
- Navigate to the `urls.js` in the frontend directory. Update the file to ensure the front end communicates effectively with the backend.
   ```sh
    cd src
    sudo nano urls.js
    export const backendUrl = "http://localhost:3000";
  ```
- Install frontend dependencies and build the React frontend
   ```sh
    sudo npm install
    sudo npm run build
   ```
   It will create a build/ folder.
- Configure Nginx to serve the build, replace the existing code with code below 
   ```sh
    sudo nano /etc/nginx/sites-available/default

   server {
    listen 80;
    server_name _;

    root /home/ubuntu/TravelMemory/frontend/build;
    index index.html index.htm;

    location / {
        try_files $uri /index.html;
     }
  }

   ```
- Restart nginx
   ```sh
    sudo systemctl restart nginx
   ```
---
**Task-3: Scaling the Application**
- Create multiple instances of both the frontend and backend servers.
   ![Picture12](https://github.com/user-attachments/assets/ea661f1e-5e57-490e-be1e-56aaa2c8fbdb)
- Add these instances to a load balancer to ensure efficient distribution of incoming traffic.
   ![Picture21](https://github.com/user-attachments/assets/ed6a2576-609a-4565-a3fe-a5b552fc8179)
   ![image](https://github.com/user-attachments/assets/5f338ff1-c8af-4772-81b1-8e137221efb2)

---

---
**Task-4: Domain Setup with Cloudflare**
- To achieve this, purchase a domain from any provider and add the domain to Cloudflare.
- Create a CNAME record pointing to the load balancer endpoint.
  ![image](https://github.com/user-attachments/assets/9c7b3dd1-d4d0-45aa-8821-4e3d29d72b9b)
- To provide a certificate to your website, go to AWS -> ACM (Certificate Manager) and request a certificate, and add those to your Cloudflare.
  ![image](https://github.com/user-attachments/assets/5ad5c714-7fbd-4c40-a68e-9f77529ebbd4)
  ![image](https://github.com/user-attachments/assets/abe87f80-3a74-49e9-96fc-7ccede8f88dc)
  ![image](https://github.com/user-attachments/assets/0cf10999-d5f4-4a65-82d7-46e391e208ba)
 --- 

 ---
**Final Output**

 ![image](https://github.com/user-attachments/assets/38b3b552-707f-42b5-bb39-17b3d9cc9753)

- Also, check in MongoDB whether the data is being inserted or not
   ![image](https://github.com/user-attachments/assets/cef93e4a-e874-40be-acb4-f261f23e0ed2)
---
---
## 🤝 Contributing
🙌 Contributions are welcome! Follow these steps:
1. Fork the repository.
2. Create a new branch:
   ```sh
   git branch new-branch
   ```
3. Commit your changes:
   ```sh
   git commit -m "Commit Message"
   ```
4. Push the code:
   ```sh
   git push origin new-branch
   ```
5. Submit a pull request.

---

## 📜 License
📄 This repository is licensed under the MIT License. See the LICENSE file for details.
