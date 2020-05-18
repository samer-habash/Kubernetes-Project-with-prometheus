Prometheus helm deploy issue :

Issue Exaplanation : 
 When restarting the PC or minikube the PV will have permissions issue .

Reason of happening:
 By default the helm uses non-root user to access the PV and write into it .
 NOTE: after successful installation the minikube hostPath PVC will have 777 mode
 - When termination the pods or restarting PC or roll out deployment-prometheus-server the mode will be the default as kubernetes with 0755
 So the pod prometheus-server will have permission denied since the docker by default uses root user to access volumes, while helm prometheus uses non-root .

Fixing the issue:
 There are multiple solutions :
 1) Go to the directory since it is hostPath in minikube ssh "/tmp/hostpath-provisioner/" and change it by chmod 777 directly
 2) Make the user run as root and change the directory permissions (added the below code in the deployment-prometheus-server) :
  Have a look at the file "fixed-deployment-prometheus-server.yaml"
  NOTE: The initContainers busybox is applied after creating the /data directory with the prometheus image

    """
    initContainers:
      - command:
        - sh
        - -c
        - chmod -R 777 /data
        image: busybox
        imagePullPolicy: Always
        name: volume-permissions
        volumeMounts:
        - mountPath: /data
          name: storage-volume
      securityContext:
        runAsUser: 0
    """
  
NOTES: 
  - I have deleted the the default code which has the "runAsUser: 65534, runAsNonRoot: true, runAsGroup: 65534, fsGroup: 65534" and replace it with just running as root user
  - First helm installation it will make the permissions of the PVC as 777 , while the write permissions will be non-root user as (nobody)
  - The files written by prometheus will be root owner , this is verstile I guess in terms of security since most of the docker containers uses non-root user (as bitnami developers)


After the fix you can restart PC, deployment, minikube and the prometheus-server will be reloading and working.

Usage of miniukube :
  - I am using minikube --vm-driver none,  since it doesn't need any port-forward and the Local Machine will be inside the cluster and also the local machine will have the /tmp/hostpath-provisioner/ directly in your /tmp/ dir.



