FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
---


FROM ubuntu:22.04
RUN apt-get update && apt-get install -y nginx
COPY index.html /var/www/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]



---

FROM ubuntu:22.04
RUN apt-get update && apt-get install -y nginx
ADD https://garam-panyachi-balti-cdec.s3.ap-south-1.amazonaws.com/index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
