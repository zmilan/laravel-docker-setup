# Main use case
This repository is created with one use case in mind: to have easy option to start developing Laravel application locally.
It is not something that you should use in production!

## Steps

- Copy all files from this project to root of your new project directory.
- In root directory, copy `.env.example` file to `.env`.
- In root directory, copy `docker-compose.override.yml.example` file to `docker-compose.override.yml`, 
and additionally adjust ports inside `docker-compose.override.yml` to your need.
- Search whole project for `{{PROJECT_NAME}}`, and replace it with your project name. Use underscore for connecting multiple words.
- Search whole project for `{{PROJECT_URL}}`, and replace it with your desired local URL. You can use same like for `{{PROJECT_NAME}}`,
just replace underscore with dash. Additionally, final local URL will have added `-dev.com`. For example if you use `test-project` for
`{{PROJECT_URL}}`, final local URL would be `http://test-project-dev.com`.
- Add final local URL to your `hosts` file (on Linux it is in `/etc/hosts`)
- You can now build and run containers with `docker-compose up --build`
- Once when containers are built and running you can access `backend` container and install laravel:
  + Run `docker exec -it {{PROJECT_NAME}}_backend bash` (replace `{{PROJECT_NAME}}` with your project name)
  + When you are inside your container, run `composer create-project laravel/laravel . `
- Once when you have installed Laravel you need to configure Vite to be able to use its dev server for hot reload
  + Run `docker exec -it {{PROJECT_NAME}}_backend bash` (replace `{{PROJECT_NAME}}` with your project name)
  + When you are inside your container, run `npm install`
  + From your editor open `src/vite.config.js` and add this part into `defineConfig(...)`:
    ```javascript
    export default defineConfig({
        server: {
            host: true,
            hmr: {
                host: '172.31.0.5', // You network address of container in docker, you can see it in output when you run `npm run dev`
            }
        },
        // ... here goes original part of config, laravel plagin...
    });
    ```
  + After that you are able to run `npm run dev` to start Vite dev server that will listen to frontend part changes and reload
  + To be able to use testing emails you need to set your Laravel `.env` file to have:
    - `MAIL_HOST="{{PROJECT_NAME}}_mailpit"`
    - `MAIL_PORT=1025`
  + To be able to use redis you need to set your Laravel `.env` file to have:
    - `REDIS_HOST="{{PROJECT_NAME}}_redis"`
- If you need to execute some command inside php container that usually require sudo, you need to access container with `docker exec -it --user root {{PROJECT_NAME}}_backend bash`

**At this moment you should have working setup. Enjoy!**

