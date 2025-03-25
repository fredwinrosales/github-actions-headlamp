
# 🔐 Crear un Token para Headlamp usando GitHub Actions

Este repositorio contiene los pasos necesarios para generar un token de acceso para la interfaz de usuario de [Headlamp](https://github.com/kinvolk/headlamp), utilizando un ServiceAccount de Kubernetes.

## 🚀 Requisitos

-   `kubectl` instalado y configurado
    
-   Acceso al cluster Kubernetes
    
-   Namespace `headlamp` existente
    

## 🛠️ Pasos

1.  **Crear un Secret asociado a un ServiceAccount**

```bash
kubectl create secret generic headlamp-token \
  --type kubernetes.io/service-account-token \
  --namespace headlamp \
  --from-literal=usage=login \
  --dry-run=client -o yaml | tee headlamp-token.yaml
```

3.  **Editar el archivo generado**

Abre el archivo `headlamp-token.yaml`:

```bash
nano headlamp-token.yaml
```

Agrega lo siguiente dentro de la sección `metadata`:

```yaml
annotations:
  kubernetes.io/service-account.name: headlamp
```

> ⚠️ Asegúrate de mantener la indentación adecuada en YAML.

3.  **Aplicar el Secret al cluster**

```bash
kubectl apply -f headlamp-token.yaml
```

4.  **Obtener el token decodificado**

```bash
kubectl get secret headlamp-token -n headlamp -o jsonpath='{.data.token}' | base64 --decode
```

Este token lo puedes usar para autenticarte en la interfaz de Headlamp.

----------

## 📌 Notas

-   Asegúrate de que el ServiceAccount `headlamp` exista en el namespace `headlamp`. Si no, créalo antes de continuar.
    
-   Este proceso no crea el ServiceAccount ni vincula roles. Solo genera el token.
    
-   Puedes usar este token para integraciones automáticas, como GitHub Actions o autenticación personalizada.
