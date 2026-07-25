```Dockerfile
FROM nginx
WORKDIR /opt
COPY . .
ADD https://templatemo.com/download/templatemo_623_novapay  ./templatemo_623_novapay.zip
RUN apt-get update && apt-get install -y unzip && unzip templatemo_623_novapay.zip
COPY ./templatemo_623_novapay/ /usr/share/nginx/html/
EXPOSE 80 
CMD ["nginx", "-g", "daemon off;"]
```
