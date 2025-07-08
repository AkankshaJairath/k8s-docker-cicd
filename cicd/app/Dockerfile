# Use the official nginx image from Docker Hub
FROM nginx:alpine

# Remove the default nginx index page
RUN rm /usr/share/nginx/html/index.html


# Copy your custom index.html to the nginx html directory
COPY index.html /usr/share/nginx/html/

RUN chmod 755 /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

#Below are the error log locations for nginx
#Access log: /var/log/nginx/access.log
#Error log: /var/log/nginx/error.log