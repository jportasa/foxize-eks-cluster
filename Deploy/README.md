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


## Estrategia versiones

Crear Tags en la rama de Master. Las imagenes se crearan con el nombre del Tag y Jenkins deployará la última versión de imagen con helm.
Los Tags tienen 3 numeros X.Y.Z    X=major, Y=minor, Z=Fix