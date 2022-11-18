- Login into VM and clone Backend code repo

  ```bash
  git clone https://github.com/ramesh-km/konamars-api
  ```

- Install dependencies and build the project

  ```bash
  # Nodjs should be installed in the system.
  npm install \
  npm run build
  ```

- Run app with pm2
  ```bash
  # Install pm2 - production grade process manager for nodejs
  sudo npm i -g pm2

  # start app with pm2
  pm2 start build/index.js
  ```

- Verify API service availability
  ```bash
  # This should return {message: 'success'}
  curl http://localhost:7000/healthcheck
  ```

- Add DNS Record for subdomain `api.<domain_name>`

- Create new nginx site

  ```bash
  sudo echo "
  server {
    server_name api.<domain_name>;
    location / {
      proxy_pass http://localhost:7000;
    }
  }
  " > /etc/nginx/sites-available/api.<domain_name>
  ```

- Enable the newly created nginx site

  ```bash
  sudo ln -s /etc/nginx/sites-available/api.<domain_name> /etc/nginx/sites-enabled/api.<domain_name>

  ```

- Run certbox again

  ```bash
  sudo certbot --nginx
  # In the process select (E) (Leave empty & just hit enter) during domain selection, nginx will issue cert for all the sites listed in nginx config
  ```

- Now API is hosted on api.<domain_name>, open your favorite api client and test the healthcheck route at `https://api.<domain_name>/healthcheck`
