# Microservices Project

## Архитектура и взаимодействие микросервисов

В проекте реализованы два основных микросервиса:

- **PlatformService** — сервис управления платформами.
- **CommandsService** — сервис управления командами, связанными с платформами.

### Взаимодействие между сервисами
В системе используются три способа коммуникации между сервисами:

#### 1. Синхронное HTTP-взаимодействие
- **PlatformService → CommandsService**
  - При создании новой платформы PlatformService отправляет HTTP POST-запрос в CommandsService по адресу `/api/c/platforms`.
  - Для этого используется класс `HttpCommandDataClient`.
  - Это позволяет PlatformService сразу синхронно передать данные о новой платформе в CommandsService.

#### 2. Синхронное gRPC-взаимодействие
- **CommandsService → PlatformService**
  - Когда CommandsService нужно получить список всех платформ, он обращается к PlatformService по gRPC (метод `GetAllPlatforms`).
  - Для этого используется proto-файл `platforms.proto` и клиент `PlatformDataClient`.

#### 3. Асинхронное взаимодействие через RabbitMQ
- **PlatformService → RabbitMQ → CommandsService**
  - При создании новой платформы PlatformService публикует событие в RabbitMQ (exchange `trigger`) с помощью класса `MessageBusClient`.
  - CommandsService подписан на этот exchange через класс `MessageBusSubscriber` и асинхронно получает события о новых платформах.
  - Это позволяет реализовать асинхронную интеграцию: PlatformService не ждёт ответа от CommandsService, а просто отправляет событие, которое будет обработано позже.

---

## Описание

В этом репозитории находятся два микросервиса (`CommandsService` и `PlatformService`) и настройки для их запуска в Kubernetes.

---

## Быстрый старт

### 1. Клонирование репозитория
```sh
# Замените ссылку на ваш форк или используйте мой оригинальный репозиторий:
git clone https://github.com/Vadyao00/Simple-Microservices.git
cd Simple-Microservices
```

### 2. Сборка Docker-образов

Перейдите в каждую папку микросервиса и соберите образы:
```sh
cd CommandsService/CommandsService
docker build -t <ваш_dockerhub_username>/commands-service:latest .
then from Simple-Microservices folder:
cd PlatformService/PlatformService
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
- vadyao0/commandservice:latest
- vadyao0/platformservice:latest

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
