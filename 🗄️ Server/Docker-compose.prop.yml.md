```yml
services:
app:
build:
context: .
dockerfile: Dockerfile
image: auranex-erp-app-production
container_name: auranex-production-app
restart: unless-stopped
working_dir: /var/www/html
volumes:
- .:/var/www/html

networks:
- auranex-production-network

nginx:
image: nginx:stable-alpine
container_name: auranex-production-nginx
restart: unless-stopped
ports:- "127.0.0.1:8002:80"
volumes:
- .:/var/www/html
- ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
networks:
- auranex-production-network
db:

image: postgres:16-alpine
container_name: auranex-production-db
restart: unless-stopped
environment:
POSTGRES_DB: auranex-production
POSTGRES_USER: auranex
POSTGRES_PASSWORD: 1jERCuHX8PYze5UXbYA2
ports:
- "5434:5432"
volumes:
- production_db_data:/var/lib/postgresql/data
networks:
auranex-production-network:
aliases:
- db-server

networks:
auranex-production-network:
driver: bridge

volumes:
production_db_data:
```