# OroCommerce Dev Environment (AAXIS Test)

## Requirements
- Git
- Docker
- Docker Compose
- At least 6 GB RAM

## 1. Clone repository
git clone git@github.com:leandropalexandregmailcom/orocommerce.git
cd orocommerce

## 2. Environment file
cp .env .env.local

Edit `.env.local`:
APP_ENV=dev
APP_DEBUG=1
ORO_DB_URL=postgresql://orocommerce:orocommerce@postgres:5432/orocommerce?sslmode=disable&charset=utf8&serverVersion=16
ORO_DB_DSN=${ORO_DB_URL}

## 3. Start services
docker compose up -d --build
docker compose ps

## 4. Install OroCommerce (inside PHP container)
docker compose exec php bash
php bin/console oro:install --env=dev --timeout=600 \
  --language=en --formatting-code=en_US \
  --organization-name="AAXIS Test" \
  --user-name=admin --user-email=admin@example.com \
  --user-firstname=Admin --user-lastname=User \
  --user-password=admin \
  --application-url="http://localhost"

## 5. Access
Storefront: http://localhost/
Backoffice: http://localhost/admin
User: admin
Password: admin

## 6. Useful commands (inside PHP container)
php bin/console cache:clear
php bin/console oro:migration:load --force --timeout=0
php bin/console oro:assets:install

## 7. Stop environment
docker compose down
docker compose down -v
