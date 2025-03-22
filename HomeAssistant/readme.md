https://github.com/k8s-at-home/charts/tree/master/charts/stable/home-assistant
helm repo add k8s-at-home https://k8s-at-home.com/charts/
helm repo update
export ADMIN_USER_PASSWORD=$(kubectl get secret --namespace "smarthome" homeassistant-postgresql -o jsonpath="{.data.admin-user-password}" | base64 -d)
export ADMIN_USER_TOKEN=$(kubectl get secret --namespace "smarthome" homeassistant-postgresql -o jsonpath="{.data.admin-user-token}" | base64 -d)
helm upgrade --install homeassistant \
                --create-namespace \
                --values values.yaml \
                --set auth.admin.token=$ADMIN_USER_TOKEN \
                --set auth.admin.password=$ADMIN_USER_PASSWORD \
                --namespace smarthome  \
                k8s-at-home/home-assistant
