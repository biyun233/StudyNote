# Catch Admin Pro

### Install

- composer

- Nodejs: v20+

- npm

- yarn

- vite

  ```sh
  # install nodejs
  cd ~
  curl -sL https://deb.nodesource.com/setup_20.x -o /tmp/nodesource_setup.sh
  sudo bash /tmp/nodesource_setup.sh
  sudo apt install nodejs
  node -v
  
  # install yarn
   sudo apt update
   echo "deb [signed-by=/etc/apt/trusted.gpg.d/yarn.gpg] https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   sudo apt install yarn
   yarn -v
  # or
  curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
  sudo apt update
  sudo apt install yarn
   
  # yarn dev报错： /bin/sh: 1: vite: not found
  # install vite
  npm install -g vite
  ```

```sh
# backend
cd cat_admin_pro
composer install
php artisan catch:install
php artisan serve

# frontend
cd web
yarn install
yarn dev
## Error: spawn xdg-open ENOENT
sudo apt-get install xdg-utils

yarn run build
## /bin/sh: 1: vue-tsc: not found
```

