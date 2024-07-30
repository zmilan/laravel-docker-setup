# Main use case
This repository is created with one use case in mind: to have easy option to start developing Laravel + Livewire/Filament application locally. 
Most services are placed inside `docker-compose.override.yml.example` as optional, in case you wish to run only Laravel. 
Since Laravel is set to SQLite by default, MySQL service is commented out by default, but you can activate it if needed.

## Steps

- Copy all files from this project to root of your new project directory.
- In root directory, copy `.env.example` file to `.env`.
- In root directory, copy `docker-compose.override.yml.example` file to `docker-compose.override.yml`, 
and additionally adjust ports inside `docker-compose.override.yml` to your need.
- Search whole project for `{{PROJECT_NAME}}`, and replace it with your project name. Use underscore for connecting multiple words.
- Search whole project for `{{PROJECT_URL}}`, and replace it with your desired local URL. You can use same like for `{{PROJECT_NAME}}`,
just replace underscore with dash. Additionally, final local URL will have added `.local`. For example for
`{{PROJECT_URL}}`, final local URL would be `http://{{PROJECT_URL}}.local`.
- Add final local URL to your `hosts` file (on Linux it is in `/etc/hosts`)
- You can now build and run containers with `docker-compose up --build`
- Once when containers are built and running you can access `backend` container and install Laravel:
  + Run `docker exec -it {{PROJECT_NAME}}_backend bash`
  + When you are inside your container, delete `.gitignore` file with command `rm -rf .gitignore`. It was there only to keep an empty folder in the git.
  + When you are inside your container, run `composer create-project laravel/laravel . `
- Once when containers are built and running you can access `frontend` container and install Quasar:
  + Run `docker exec -it {{PROJECT_NAME}}_frontend bash`
  + When you are inside your container, delete `.gitignore` file with command `rm -rf .gitignore`. It was there only to keep an empty folder in the git.
  + When you are inside your container, run `npm init quasar` and follow Quasar initialization procedure. For directory name choose `.`.
- To be able to use testing emails you need to set your Laravel `.env` file to have:
  + `MAIL_HOST="{{PROJECT_NAME}}_mailpit"`
  + `MAIL_PORT=1025`
- To be able to use redis you need to set your Laravel `.env` file to have:
  + `REDIS_HOST="{{PROJECT_NAME}}_redis"`
- If you need to execute some command inside php container that usually require sudo, you need to access container with `docker exec -it --user root {{PROJECT_NAME}}_backend bash`
- Each time when you `up` you need to start Quasar in the `frontend` container:
  + Run `docker exec -it {{PROJECT_NAME}}_frontend bash`
  + When inside of docker container run `quasar dev -p 8383`. You can adjust port in case that it is changed.

**At this moment you should have working setup. Enjoy!**

