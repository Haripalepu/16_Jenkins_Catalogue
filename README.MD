*** Catalogue 
It was written in Node js
Before doing this the Project infra should be ready VPC,SUBNET,SG,VPN,aPP_ALB,DATABSES
first we have to build 01_catalogue_CI then 02_catalogue_CD in jenkins.
01_catalogue_CI contains application code and using CI we are creating an artifact

From terraform we should send the artifact version to ansible.