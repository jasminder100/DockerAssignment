FROM nginx:latest

# Nginx logs will go to /var/log/nginx (default)
# We expose port 80

# Remove the symlinks and make real log files
RUN unlink /var/log/nginx/access.log && \
    unlink /var/log/nginx/error.log && \
    touch /var/log/nginx/access.log && \
    touch /var/log/nginx/error.log && \
    chown nginx:nginx /var/log/nginx/access.log && \
    chown nginx:nginx /var/log/nginx/error.log

EXPOSE 80
