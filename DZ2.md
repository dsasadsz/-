```mermaid
componentDiagram
    %% --- ВНЕШНИЕ СИСТЕМЫ ---
    component "Внешние Курьерские Службы\n(External Logistics Providers)" as ExtCouriers
    component "Платежный Шлюз\n(Payment Gateway)" as PaymentSys

    %% --- FRONTEND ---
    package "Frontend Layer" {
        component "Web Application\n(Клиенты)" as WebApp
        component "Mobile Application\n(Курьеры)" as MobApp
    }

    %% --- BACKEND ---
    package "Backend System" {
        component "Backend Core\n(Order Management)" as Backend
        
        component "Модуль Склада\n(Warehouse Mgmt)" as Warehouse
        component "Модуль Оптимизации\nМаршрутов" as RouteOpt
        component "Система Уведомлений\n(Notification Service)" as Notify
        component "Модуль Интеграции\nс Курьерами" as CourierAdapter
        component "Система Аналитики" as Analytics
        
        database "База Данных\n(DB)" as DB
    }

    %% --- ИНТЕРФЕЙСЫ И СВЯЗИ ---

    %% Frontend -> Backend
    interface "REST API / WebSocket" as I_API
    Backend -- I_API
    WebApp --> I_API : HTTPS
    MobApp --> I_API : HTTPS

    %% Backend -> Database
    interface "JDBC / SQL" as I_DB
    DB -- I_DB
    Backend --> I_DB : Чтение/Запись
    Warehouse --> I_DB : Обновление остатков

    %% Backend -> Modules
    %% Склад
    interface "Inventory API" as I_Inv
    Warehouse -- I_Inv
    Backend --> I_Inv : Резерв товаров

    %% Маршрутизация
    interface "Route Calc Interface" as I_Route
    RouteOpt -- I_Route
    Backend --> I_Route : Запрос расчета

    %% Уведомления
    interface "Message Queue / Event Bus" as I_Msg
    Notify -- I_Msg
    Backend --> I_Msg : Триггер события

    %% Аналитика
    interface "Data Stream / Report API" as I_Analytics
    Analytics -- I_Analytics
    Backend --> I_Analytics : Логирование событий

    %% Интеграция с курьерами
    interface "Internal Logistics API" as I_Logist
    CourierAdapter -- I_Logist
    Backend --> I_Logist : Делегирование доставки

    %% Внешние связи
    %% Курьеры
    interface "External API / Webhooks" as I_ExtAPI
    ExtCouriers -- I_ExtAPI
    CourierAdapter --> I_ExtAPI : Sync/Async запросы

    %% Платежи
    interface "Payment API" as I_Pay
    PaymentSys -- I_Pay
    Backend --> I_Pay : Обработка транзакций


 ```
