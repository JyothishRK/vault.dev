
---

`gradle build:`To Build it which can be deployable.

Once build, you need to upload to server.  
Here is the command:

scp -i non-prod.pem Backend/vc-syncfusion/java-2023/build/libs/ej2-webservices-0.0.5-SNAPSHOT.jar ec2-user@3.7.199.173:/tmp/`

Move JAR from tmp to lib folder
sudo mv ej2-webservices-0.0.5-SNAPSHOT.jar /home/ec2-user/java-2023/build/libs/

Once uploaded,  
You can run this command to run:`nohup java -jar java-2023/build/libs/ej2-webservices-0.0.15-SNAPSHOT.jar`

---

## Current AWS Setup:

