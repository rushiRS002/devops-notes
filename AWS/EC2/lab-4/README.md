# DOC
- https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-predictive-scaling.html ---> Predictive scalling 
- https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html ---> Shedule scalling 
- https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html ---> Dynamic scalling 



## The EC2 Auto Scaling service enables automatically launching EC2 instances based on pre-defined conditions named scaling policies. A common scenario for auto scaling is launching more instances to cope with a sudden demand increase.

## Launch Template: it allows creating EC2 configurations so the service knows what type of EC2 instance to create when needed;
Auto Scaling Groups: a logical group of EC2 instances controlled by the auto scaling service.

# Architecture diagram 
![ASG with ALB](../images/asg-alb.png)


# Steps to create an Auto Scalling Group (ASG)

## step 1 - create an asg give name to asg 
![step1](./images/step1.png)

## step 2 - select an avalibility zone to lanch an ec2 instances 

![step2](./images/step2.png)


## step 3 - attach an Load balancer to test the load balancing on server / domain 

![step3](./images/step3.png)

## step 4 - crete and target group 
![step4](./images/step4.png)

## step 5 - give an cpacity to lanch an ec2 instances 
```
- desired capacity - capacity which is matched by number of ec2 instances
- min capacity - if no traffic is comming on server still this many servers are going to be active 
- max capacity - if server are incresing and traffic is increasing still not crosing the maximum capacity 
```
![step5](./images/step5.png)

## step 6 - attach target traccing to target group check the ec2 instance utilization 
![step6](./images/step6.png)

## step 7 - reviev and crete asg 
![step7](./images/step7.png)

## varify asg is created or not 

![step8](./images/step8.png)

## to increase load on severs use stress command 
- To install use `apt install stress -y`
- to run stress use example and run in baground using &
- example `stress --cpu 8 --io 4 --vm 2 --vm-bytes 128M --timeout 1000s &`
![step10](./images/step10.png)

## check new instance are scalled or not 
![step11](./images/step11.png)

![step12](./images/step12.png)

## check for ec2 instance are getting decreased as traffice lowers 
![step13](./images/step13.png)



![step14](./images/step14.png)



---

# clean up 
## delete asg to delete all the resource and make shure to chek Load Balancer is deleted or not 
![step15](./images/step15.png)

## delete load balancer 
![step16](./images/step16.png)

## delete target group 

![step18](./images/step18.png)





