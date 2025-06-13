# Microservices Project

## Описание

В этом репозитории находятся два микросервиса (`CommandsService` и `PlatformService`) и настройки для их запуска в Kubernetes.

---

## Быстрый старт

### 1. Клонирование репозитория
```sh
git clone https://github.com/ВАШ_ПОЛЬЗОВАТЕЛЬ/ВАШ_РЕПОЗИТОРИЙ.git
cd Microservices
```

### 2. Сборка Docker-образов

Перейдите в каждую папку микросервиса и соберите образы:
```sh
cd CommandsService
docker build -t <ваш_dockerhub_username>/commands-service:latest .
cd ../PlatformService
docker build -t <ваш_dockerhub_username>/platform-service:latest .
```

### 3. Публикация образов на Docker Hub

```sh
docker login
# Замените <ваш_dockerhub_username> на ваш логин Docker Hub
docker push <ваш_dockerhub_username>/commands-service:latest
docker push <ваш_dockerhub_username>/platform-service:latest
```

### 4. Деплой в Kubernetes

Перейдите в папку с yaml-файлами:
```sh
cd ../K8S
kubectl apply -f .
```

### 5. Проверка работы

Проверьте, что все поды запущены:
```sh
kubectl get pods
```

#### (Опционально) Примеры curl-запросов для тестирования сервисов

---

## Структура проекта
- `CommandsService/` — исходный код и Dockerfile для сервиса команд
- `PlatformService/` — исходный код и Dockerfile для сервиса платформ
- `K8S/` — Kubernetes-манифесты (deployment, service и т.д.)

---

## Требования
- Docker
- Kubernetes (minikube/kind/другой кластер)
- kubectl

---

## TODO
- [ ] Указать ваш Docker Hub username
- [ ] Уточнить тип кластера Kubernetes
- [ ] Добавить примеры curl-запросов для тестирования 