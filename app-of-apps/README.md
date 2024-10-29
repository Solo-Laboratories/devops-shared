#  App of Apps
It's an application that manages your applications. Meaning you don't have to manually push chart updates but rather just push to your repository from anywhere and it will update your application(s). Who watches the watchmen?

## Installation
```bash
kubectl apply -f app-of-apps.yaml
```
... that's it. Sit back and watch it all explode into existence!

## Updating
Just re-apply the app-of-apps.yaml file like you did in the installation.

## Notes
You just have to update the applications in the directory you specified and it will catch and automatically update.

That said, if you want to delete the child resources you have to add an annotation to your application file.

Let me provide an example. I have a folder structure of 'k8s/active' and 'k8s/deprecated'. If I want to install gitea, I move the gitea folder holding my application.yaml file to the 'active' folder and push the changes. My app of apps will catch it and install gitea for me automatically. Now to delete gitea, I move it back to my deprecated folder. However, it won't delete gitea. It will delete the application and not the child resources.

So, if you want to delete the child resources, you need to add `metadata.finalizers=[resources-finalizer.argocd.argoproj.io]` to the application. Using the example above, you want to add it to the gitea application. This will delete the resources.

If you want to create a namespace during deployment of the application, you need to add `spec.syncPolicy.syncOptions=[CreateNamespace=true]` to the application. 