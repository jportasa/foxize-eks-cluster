# Deployments

## Deploy y Rollback con helm

Pasar un comprobador de sintaxys de helm:
```
helm lint ./myapp
```
Despliegue sin downtime de nueva imagen:
```
helm upgrade --install my-app --set ....
```
Ver histórico de despliegues con helm
```
helm history my-todo-app
```
Ver diferencias
```
helm diff upgrade
```
Rollback de versión
```
helm rollback my-todo-app <version number>
```

