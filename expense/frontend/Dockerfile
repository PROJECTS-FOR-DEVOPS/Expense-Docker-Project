FROM nginx:stable-alpine3.20-perl

# Remove default nginx content
RUN rm -rf /usr/share/nginx/html/*

# Copy your app
COPY --chown=nginx:nginx . /usr/share/nginx/html/

# Copy custom nginx config
COPY --chown=nginx:nginx nginx.conf /etc/nginx/nginx.conf

# Create cache directories with proper ownership
RUN mkdir -p /var/cache/nginx/client_temp \
             /var/cache/nginx/proxy_temp \
             /var/cache/nginx/fastcgi_temp \
             /var/cache/nginx/uwsgi_temp \
             /var/cache/nginx/scgi_temp && \
    touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/cache/nginx /var/log/nginx /var/run/nginx.pid

# Switch to nginx user
USER nginx

# Expose port and start nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
