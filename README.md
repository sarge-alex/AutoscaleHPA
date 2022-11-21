# AutoScale with HPA
В соот-и с заданием:
- у нас мультизональный кластер (три зоны), в котором пять нод
- приложение требует около 5-10 секунд для инициализации
- по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
- на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU. 
  По памяти всегда “ровно” в районе 128M memory
- приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
- хотим максимально отказоустойчивый deployment
- хотим минимального потребления ресурсов от этого deployment’а

Файлы предоставлены не для использования в production кластере, тестирование не проведено.
Только демонстрация алгоритма решения поставленной задачи.

Run this in a separate terminal
so that the load generation continues and you can carry on with the rest of the steps
```
kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://nginx-deployment; done"
```
type Ctrl+C to end the watch when you're ready
```
kubectl get hpa nginx-deployment --watch
```
As a result, the Deployment was resized to 7 replicas:
```
kubectl get deployment nginx-deployment
```
**examples:**

https://www.kubecost.com/kubernetes-autoscaling/kubernetes-hpa/
https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
