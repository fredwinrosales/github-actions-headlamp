
# 🛠️ Guía de Verificación y Debug para el Despliegue de Headlamp en Kubernetes

Si Headlamp no aparece disponible tras ejecutar el workflow de GitHub Actions, sigue estos pasos para verificar el estado de la instalación y solucionar posibles problemas.

----------

## ✅ Paso 1: Verificar que el namespace fue creado

Ejecuta el siguiente comando para asegurarte de que el namespace `headlamp` existe en el cluster:

```bash
kubectl get ns
```

Si no aparece, revisa tu archivo `k8s/namespace.yaml` y asegúrate de que esté correctamente definido.

----------

## ✅ Paso 2: Confirmar que Helm instaló el release

Verifica si Helm registró correctamente el release de Headlamp:

```bash
helm list -n headlamp
```

> Si no ves el release, puede que Helm no haya desplegado nada. Asegúrate de que no hubo errores en el paso de instalación.

----------

## ✅ Paso 3: Validar los recursos desplegados

Comprueba si los pods están corriendo en el namespace `headlamp`:

```bash
```
kubectl get pods -n headlamp
```

Para ver todos los recursos creados (pods, servicios, deployments, etc):

```bash
kubectl get all -n headlamp
```

----------

## ✅ Paso 4: Revisar los logs del workflow en GitHub Actions

1.  Ve a la sección **Actions** del repositorio.
    
2.  Abre la ejecución más reciente del workflow.
    
3.  Revisa el paso **`Install/Upgrade Headlamp`**.
    
4.  Busca errores silenciosos o warnings que puedan haber impedido la instalación.
    

> ⚠️ Helm puede fallar sin detener el workflow si hay errores en los valores del chart.

----------

## ✅ Paso 5: Validar el archivo `k8s/helm-values.yaml`

Verifica que los valores definidos en `helm-values.yaml` sean compatibles con el chart de Headlamp.

Como prueba rápida, intenta instalar sin valores personalizados:

```bash
helm upgrade --install headlamp headlamp/headlamp -n headlamp --create-namespace
```
----------

## ✅ Paso 6: Ejecutar Helm con salida detallada (debug)

Para obtener una salida más detallada de la instalación, usa la bandera `--debug`:

```bash
helm upgrade --install headlamp headlamp/headlamp -n headlamp --create-namespace --debug
```

Esto te mostrará detalles sobre cómo se renderizan los recursos, lo cual es muy útil si hay errores en los valores o la plantilla del chart.
