[Project__session](https://github.com/mukundDeo9325/student-app-k8s.git)

[refrance_doc](https://github.com/mukundDeo9325/student-app-k8s/blob/main/Kubernetes/cdec-studentapp-k8s.md)



```
1  git clone https://github.com/mukundDeo9325/student-app-k8s.git
    2  ls
    3  cd student-app-k8s/q
    4  cd student-app-k8s
    5  ls
    6  cd backend/
    7  ls
    8  vim src/main/resources/application.properties 
    9  ls
   10  docker build -t mukunddeo9325/cdecbackend .
   11  docker images 
   12  docker login 
   13  ls
   14  cd ../frontend/
   15  ls
   16  vim .env 
   17  docker images
   18  docker push mukunddeo9325/cdecbackend:latest  
   19  ls
   20  docker images
   21  cd ../backend/
   22  ls
   23  vim backend.yaml 
   24  ls
   25  kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
   26  ls
   27  kubectl apply -f backend.yaml 
   28  kubectl get pods
   29  kubectl get svc
   30  cd ../frontend/
   31  ls
   32  docker build -t mukunddeo9325/cdecfrontend .
   33  docker images
   34  docker push mukunddeo9325/cdecfrontend:latest
   35  ls
   36  vim frontend.yaml 
   37  ls
   38  kubectl apply -f frontend.yaml 
   39  kubectl get pods
   40  kubectl get pods -w
   41  kubectl get svc
   42  kubectl get ns
   43  kubectl get svc -n ingress-nginx
   44  cd ..
   45  ls
   46  cd student-app-k8s/
   47  ls
   48  kubectl apply -f ingress.yaml
   49  history 
   50  histroy >> file.txt
   51  history >> file.txt

```
## node server 
```
yum update
yum install mysql-client -y
sudo yum install mariadb105-server -y
mysql -h <database-endpoint> -u <username> -p<password>
CREATE DATABASE student_db;
```



