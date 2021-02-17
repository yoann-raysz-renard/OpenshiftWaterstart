# Pacman Web edition

**Note: the content of the *app* folder is a copy of https://github.com/uzyexe/pacman** 


to deploy pacman on your okd cluster, retrieve a token from the web ui and connect with the oc CLI.
```
oc login -u kubeadmin -p APBEh-jjrVy-hLQZX-VI9Kg https://api.crc.testing:6443
```

create a project to host our pacman application
```
oc new-project pacman-project
```


Build and push the pacman image
```
cd PacmanDeploy/app;
docker build -t pacman:latest . ;
docker tag pacman:latest default-route-openshift-image-registry.apps-crc.testing/pacman-project/pacman;
docker push default-route-openshift-image-registry.apps-crc.testing/pacman-project/pacman;
```

deploy pacman using the openshift template
```
cd PacmanDeploy/pacman.yaml;
oc process -f pacman.yaml | oc apply -f - -n pacman-project
```

Check if the new pacman pod is running 
```
oc get pods -w
```

if the pod is running, you can connect to it using the dedicated route, e.g. *https://pacman-pacman-project.apps-crc.testing*

If an issued occured, try to retrieve logs or the deployment pod or the application pod from the web console or the commande line.
```
oc get pods;
oc logs <podname>;
```

you could also try to start the pod in debug mode:
```
oc debug <podname>
```