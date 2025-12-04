@startuml
useCaseDiagram
    actor "Гость" as Guest
    actor "Студент" as Student
    actor "Преподаватель" as Instructor
    actor "Администратор" as Admin

    package "Система управления курсами" {
        useCase "Регистрация" as UC1
        useCase "Подтверждение почты" as UC1_1
        useCase "Авторизация" as UC2
        
        useCase "Просмотр списка курсов" as UC3
        useCase "Поиск и фильтрация" as UC3_1
        useCase "Запись на курс" as UC4
        useCase "Прохождение теста" as UC5
        useCase "Оставить отзыв" as UC6
        
        useCase "Создание/Ред. курса" as UC7
        useCase "Добавление материалов" as UC8
        useCase "Создание тестов" as UC9
        useCase "Модерация отзывов" as UC10
        
        useCase "Управление аккаунтами" as UC11
        useCase "Просмотр аналитики" as UC12
    }

    %% Наследование ролей (Иерархия)
    Guest <|-- Student : Становится после входа
    Student <|-- Instructor : Расширяет права
    Student <|-- Admin : Расширяет права

    %% Связи Гостя
    Guest --> UC1
    Guest --> UC2
    
    %% Связи Студента
    Student --> UC3
    Student --> UC4
    Student --> UC5
    Student --> UC6

    %% Связи Преподавателя
    Instructor --> UC7
    Instructor --> UC8
    Instructor --> UC9
    Instructor --> UC10

    %% Связи Админа
    Admin --> UC11
    Admin --> UC12

    %% Includes и Extends
    UC1 ..> UC1_1 : <<include>>
    UC3 <.. UC3_1 : <<extend>>
    UC7 ..> UC8 : <<include>>
    UC7 ..> UC9 : <<include>>

@enduml
