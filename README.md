# Microservices Project

## Описание

В этом репозитории находятся два микросервиса (`CommandsService` и `PlatformService`) и настройки для их запуска в Kubernetes.

---

## Быстрый старт

### 1. Клонирование репозитория
```sh
# Замените ссылку на ваш форк или используйте мой оригинальный репозиторий:
git clone https://github.com/vadyao0/Microservices.git
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
# Пушим образы в ваш Docker Hub (замените <ваш_dockerhub_username> на ваш логин)
docker push <ваш_dockerhub_username>/commands-service:latest
docker push <ваш_dockerhub_username>/platform-service:latest
```

**ИЛИ** используйте уже готовые образы из моего Docker Hub:
- vadyao0/commands-service:latest
- vadyao0/platform-service:latest

Для этого просто не меняйте image в yaml-файлах K8S.

### 4. Деплой в Kubernetes (minikube/kind)

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

#### Получение доступа к Ingress

Добавьте в hosts:
- **Windows:** Откройте файл `C:\Windows\System32\drivers\etc\hosts` от имени администратора и добавьте строку:
  ```
  127.0.0.1 acme.com
  ```
- **Linux/Mac:**
  ```
  sudo -- sh -c "echo '127.0.0.1 acme.com' >> /etc/hosts"
  ```

---

## Тестирование сервисов

**PlatformService:**
```sh
curl -X GET http://acme.com/api/platforms
```

**CommandsService:**
```sh
curl -X GET http://acme.com/api/c/platforms
```

---

## Структура проекта
- `CommandsService/` — исходный код и Dockerfile для сервиса команд
- `PlatformService/` — исходный код и Dockerfile для сервиса платформ
- `K8S/` — Kubernetes-манифесты (deployment, service, ingress и т.д.)

---

## Требования
- Docker
- Kubernetes (minikube/kind)
- kubectl

--- 