# DevOps Проект: Nginx + PostgreSQL с использованием Docker

Этот проект демонстрирует настройку и запуск сервисов Nginx и PostgreSQL с использованием Docker. Nginx настроен так, чтобы принимать только `POST` запросы, а PostgreSQL контейнер предварительно конфигурируется для создания пользователя и базы данных с именем `test`.

## Содержание
1. [Структура проекта](#структура-проекта)
2. [Требования](#требования)
3. [Как запустить проект](#как-запустить-проект)
4. [Тестирование Nginx](#тестирование-nginx)
5. [Тестирование PostgreSQL](#тестирование-postgresql)
6. [Остановка проекта](#остановка-проекта)
7. [Решение проблем](#решение-проблем)

---

## Структура проекта

```
project/
├── nginx/
│   ├── Dockerfile          # Dockerfile для Nginx
│   └── nginx.conf          # Конфигурационный файл Nginx
├── postgresql/
│   └── Dockerfile          # Dockerfile для PostgreSQL
└── docker-compose.yml      # Файл конфигурации Docker Compose
```

### **nginx.conf**
Файл конфигурации Nginx содержит директиву для блокировки всех запросов, кроме `POST`:

```nginx
limit_except POST {
    deny all;
}
```

### **Настройка PostgreSQL**
Контейнер PostgreSQL создаётся с использованием следующих параметров:
- **Пользователь:** `test`
- **Пароль:** `test_password`
- **База данных:** `test`

---

## Требования
- **Docker**: Убедитесь, что Docker установлен и запущен.
- **Docker Compose**: Поставляется с Docker Desktop или устанавливается отдельно.

---

## Как запустить проект

1. Склонируйте репозиторий и перейдите в директорию проекта:
   ```bash
   git clone https://github.com/zagir-dg/devops_hw_1.git
   cd devops_mipt
   ```

2. Соберите и запустите сервисы:
   ```bash
   docker-compose up --build
   ```

3. Проверьте, что контейнеры запущены:
   ```bash
   docker ps
   ```

---

## Тестирование Nginx

### **1. Проверьте GET-запрос (должен быть запрещён):**
```bash
curl -X GET http://localhost:8080
```
**Ожидаемый результат:** Ошибка `403 Forbidden`.

### **2. Проверьте POST-запрос (должен быть разрешён):**
```bash
curl -X POST http://localhost:8080
```
**Ожидаемый результат:** Ответ от сервера 405(не запрещён, но не настроен для работы).

---

## Тестирование PostgreSQL

1. Подключитесь к базе данных PostgreSQL через контейнер:
   ```bash
   docker exec -it postgres-container psql -U test -d test
   ```

2. Выполните следующие SQL-команды для проверки:
   - Создайте таблицу:
     ```sql
     CREATE TABLE example (id SERIAL PRIMARY KEY, name VARCHAR(50));
     ```
   - Добавьте данные:
     ```sql
     INSERT INTO example (name) VALUES ('First Entry'), ('Second Entry');
     ```
   - Просмотрите данные:
     ```sql
     SELECT * FROM example;
     ```

3. Выйдите из PostgreSQL:
   ```bash
   \q
   ```

---

## Остановка проекта
Для остановки и удаления контейнеров выполните:
```bash
docker-compose down
```


## Лицензия
Проект распространяется под лицензией [MIT License](LICENSE).

---
