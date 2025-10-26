FROM yourls:1.10.2-apache

# Build argument for production optimizations
ARG PRODUCTION=false

# Copy Apache configuration
COPY apache/ports.conf /etc/apache2/ports.conf
COPY apache/vhost.conf /etc/apache2/sites-available/000-default.conf

# Set proper permissions for YOURLS (YOURLS will create its own .htaccess)
RUN chown -R www-data:www-data /var/www/html && \
    chmod 755 /var/www/html

# Enable required Apache modules
RUN a2enmod rewrite headers

# Production-only optimizations
RUN if [ "$PRODUCTION" = "true" ]; then \
    echo "ServerTokens Prod" >> /etc/apache2/apache2.conf && \
    echo "ServerSignature Off" >> /etc/apache2/apache2.conf; \
    fi

EXPOSE 9001
