# kubernetis-dz02
«Базовые объекты K8S»

Проверим среду окружения.

MicroK8s запущен и готов
![alt text](image.png)

Запишем конфиг из MicroK8s
![alt text](image-1.png)

Проверяем или устанввливаем права
![alt text](image-2.png)

Проверим доступность нашего описанного кластера:
```
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
```
Все отлично:
![alt text](image-3.png)


---=== Приступаем к практике ===---

Задание 1.
Создать Pod с именем hello-world
Создать манифест (yaml-конфигурацию) Pod: [text](hello-world-pod.yml)

![alt text](image-4.png)

Использовать image - gcr.io/kubernetes-e2e-test-images/echoserver:2.2.

* указали в манифесте

Создаем pod
'''
kubectl apply -f hello-world-pod.yml
'''
![alt text](image-5.png)

Проверяем pod
![alt text](image-6.png)


Подключиться локально к Pod с помощью kubectl port-forward и вывести значение (curl или в браузере).

Выполним настройку сети кубера для работы с подом через порт 8080
```
kubectl port-forward pod/hello-world 8080:8080
```
Здесь локальный порт 8080 пробрасывается напрямую на порт 8080 внутри контейнера.


![alt text](image-7.png)

Проверяем подключением через браузер:
![alt text](image-8.png)

Проверяем подключением из терминала
```
curl --verbose --trace-ascii - http://127.0.0.1:8080/

```
![alt text](image-9.png)
![alt text](image-10.png)

```
sudo tcpdump -i lo port 8080 -nn -X
```

![alt text](image-11.png)


Задание 2. 
Создать Service и подключить его к Pod
Создали новый под и указали в нем метку
Манифест Pod: [text](netology-web.yaml)

Создать Pod с именем netology-web.
Использовать image — gcr.io/kubernetes-e2e-test-images/echoserver:2.2.

Создать Service с именем netology-svc и подключить к netology-web.
Подключиться локально к Service с помощью kubectl port-forward и вывести значение (curl или в браузере).
Создали манифест для сервиса и подключили его с помощью селектора к поду.
Манифест сервиса: [text](netology-svc.yaml)

Применяем оба манифеста:

```
kubectl apply -f netology-web.yaml
kubectl apply -f netology-svc.yaml
```
![alt text](image-12.png)

Внутри кластера подключение к поду идет через сервис, который на 80 порту, отправляет на 8080 порт пода.

Произведем публикацию локального порта 8081 внутрь кластера на сервис на порт 80.
```
kubectl port-forward svc/netology-svc 8081:80
```

Localhost:8081 ---> Service:80 ---> Pod:8080

![alt text](image-13.png)

Проверяем подключение:

![alt text](image-14.png)
![alt text](image-15.png)

![alt text](image-16.png)

![alt text](image-17.png)

Спасибо, с уважением, Виктор
