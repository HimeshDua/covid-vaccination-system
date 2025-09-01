# COVID Test and Vaccination System - Installation Guide

## System Requirements

### Server Requirements
- PHP >= 8.2
- Composer
- SQLite/MySQL/PostgreSQL
- Web Server (Apache/Nginx)

### PHP Extensions Required
- BCMath PHP Extension
- Ctype PHP Extension
- cURL PHP Extension
- DOM PHP Extension
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PCRE PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- ZIP PHP Extension

## Installation Steps

### 1. Clone or Download the Project
```bash
git clone <repository-url>
cd covid-vaccination-system
```

### 2. Install Dependencies
```bash
composer install
```

### 3. Environment Configuration
```bash
# Copy environment file
cp .env.example .env

# Generate application key
php artisan key:generate
```

### 4. Database Setup

#### For SQLite (Recommended for Development)
```bash
# Create database file
touch database/database.sqlite

# Update .env file
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database/database.sqlite
```

#### For MySQL (Production)
```bash
# Update .env file
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=covid_vaccination_system
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 5. Run Migrations and Seeders
```bash
# Run migrations
php artisan migrate

# Seed database with sample data
php artisan db:seed
```

### 6. Start Development Server
```bash
php artisan serve
```

The application will be available at `http://localhost:8000`

## Default Login Credentials

### Admin Access
- **URL:** `http://localhost:8000/admin/login`
- **Email:** admin@covidvaccination.com
- **Password:** admin123

### Hospital Access
- **URL:** `http://localhost:8000/hospital/login`
- **Credentials:** Use any of the seeded hospital accounts or register a new one
- **Note:** Hospital accounts need admin approval before they can access the system

### Patient Access
- **URL:** `http://localhost:8000/patient/login`
- **Credentials:** Use any of the seeded patient accounts or register a new one
- **Sample Patient:**
  - Email: john.doe@email.com
  - Password: password123

## Configuration

### Application Settings
Edit the `.env` file to configure:

```env
APP_NAME="COVID Vaccination System"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

# Database Configuration
DB_CONNECTION=sqlite
DB_DATABASE=/workspace/database/database.sqlite

# Session Configuration
SESSION_DRIVER=database
SESSION_LIFETIME=120

# Mail Configuration (for notifications)
MAIL_MAILER=log
MAIL_FROM_ADDRESS="noreply@covidvaccination.com"
MAIL_FROM_NAME="${APP_NAME}"
```

### Web Server Configuration

#### Apache (.htaccess)
```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^(.*)$ public/$1 [L]
</IfModule>
```

#### Nginx
```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /path/to/covid-vaccination-system/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

## Troubleshooting

### Common Issues

#### 1. Database Connection Error
**Error:** `Database connection [mysql] not configured`
**Solution:** 
- Check your `.env` file database settings
- Ensure database server is running
- Verify credentials are correct

#### 2. Migration Errors
**Error:** `Table already exists`
**Solution:**
```bash
php artisan migrate:reset
php artisan migrate
```

#### 3. Permission Errors
**Error:** `The stream or file could not be opened`
**Solution:**
```bash
chmod -R 755 storage
chmod -R 755 bootstrap/cache
```

#### 4. Composer Dependencies
**Error:** `Class not found`
**Solution:**
```bash
composer dump-autoload
```

#### 5. Session Issues
**Error:** `Session store not set on request`
**Solution:**
- Ensure `SESSION_DRIVER=database` in `.env`
- Run `php artisan migrate` to create session table

### Performance Optimization

#### 1. Config Caching
```bash
php artisan config:cache
```

#### 2. Route Caching
```bash
php artisan route:cache
```

#### 3. View Caching
```bash
php artisan view:cache
```

#### 4. Optimize Autoloader
```bash
composer install --optimize-autoloader --no-dev
```

## Security Considerations

### Production Deployment

1. **Environment Security**
   ```env
   APP_ENV=production
   APP_DEBUG=false
   ```

2. **HTTPS Configuration**
   - Enable SSL/TLS certificates
   - Configure secure headers
   - Update APP_URL to use https://

3. **Database Security**
   - Use strong database passwords
   - Limit database user permissions
   - Enable database encryption

4. **File Permissions**
   ```bash
   chmod -R 755 /path/to/application
   chmod -R 644 /path/to/application/storage
   chmod -R 644 /path/to/application/bootstrap/cache
   ```

### Backup Strategy

#### Database Backup
```bash
# SQLite
cp database/database.sqlite backup/database_$(date +%Y%m%d).sqlite

# MySQL
mysqldump -u username -p database_name > backup/database_$(date +%Y%m%d).sql
```

#### File Backup
```bash
tar -czf backup/application_$(date +%Y%m%d).tar.gz \
    --exclude='node_modules' \
    --exclude='vendor' \
    --exclude='storage/logs' \
    .
```

## Maintenance

### Regular Tasks

#### Daily
- Monitor error logs
- Check system performance
- Verify backup completion

#### Weekly
- Update dependencies
- Review security logs
- Performance analysis

#### Monthly
- Database optimization
- Security audit
- Documentation updates

### Log Management
```bash
# Clear application logs
php artisan log:clear

# View recent errors
tail -f storage/logs/laravel.log
```

## Support and Contact

For technical support or questions about the COVID Test and Vaccination System:

- **Documentation:** Refer to this guide and PROJECT_DOCUMENTATION.md
- **Code Comments:** All methods and classes are thoroughly documented
- **Issue Tracking:** Use the project repository issue tracker

---

**Last Updated:** September 2025  
**Version:** 1.0.0  
**Laravel Version:** 12.x  