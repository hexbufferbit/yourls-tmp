FROM yourls:1.10.2-apache

# Build argument for production optimizations
ARG PRODUCTION=false

# Copy Apache configuration
COPY apache/ports.conf /etc/apache2/ports.conf
COPY apache/vhost.conf /etc/apache2/sites-available/000-default.conf

# Copy .htaccess file for URL rewriting
COPY htaccess /var/www/html/.htaccess

# Set proper permissions for YOURLS
RUN chown -R www-data:www-data /var/www/html && \
    chmod 664 /var/www/html/.htaccess

# Enable required Apache modules
RUN a2enmod rewrite headers

# Production-only optimizations
RUN if [ "$PRODUCTION" = "true" ]; then \
    echo "ServerTokens Prod" >> /etc/apache2/apache2.conf && \
    echo "ServerSignature Off" >> /etc/apache2/apache2.conf; \
    fi

EXPOSE 9001
