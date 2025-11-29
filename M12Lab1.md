 Основные элементы:
  Участники (Swimlanes): Диаграмма разделена на 5 зон ответственности: Руководитель, HR, Система, Кандидат и IT. Это позволяет четко видеть, кто за что отвечает.
  Начало и конец: Обозначены оранжевыми кругами (Начало, Конец).

Логика ветвлений (Decisions):
На диаграмме представлены ключевые точки принятия решений (ромбы):

  Валидация заявки: Циклическая проверка. Если HR отклоняет заявку, процесс возвращается к Руководителю через уведомление Системы.
  Скрининг резюме: Если кандидат не подходит на этапе анкеты, Система отправляет автоматический отказ.
  Результат собеседований: Проверка происходит после этапов HR и Технического интервью. Если любой из них провален, процесс идет к отказу.
  Решение кандидата: Кандидат может отказаться от оффера даже после успешного прохождения всех этапов.

Параллельные действия (Fork/Join):
В финальной части использован паттерн Fork (черный круг разветвления), когда кандидат принимает оффер. Запускаются два параллельных процесса:
  Система: Добавляет сотрудника в базу данных.
  HR и IT: HR уведомляет IT, и IT начинает настройку рабочего места. Эти действия происходят одновременно и сходятся в точке Join перед завершением процесса.

```mermaid
graph TD
    %%  СТИЛИЗАЦИЯ
    classDef startend fill:#f96,stroke:#333,stroke-width:2px,rx:10,ry:10;
    classDef process fill:#fff,stroke:#333,stroke-width:1px;
    classDef decision fill:#ff9,stroke:#333,stroke-width:1px,rhombus;
    classDef artifact fill:#eef,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5;

    %%  НАЧАЛО ПРОЦЕССА 
    Start((Начало)):::startend --> M_CreateReq

    %%  ДОРОЖКА: РУКОВОДИТЕЛЬ 
    subgraph Manager_Lane [Руководитель отдела]
        direction TB
        M_CreateReq[Создать заявку на вакансию]:::process
        M_FixReq[Доработать заявку]:::process
        M_TechInterview[Техническое собеседование]:::process
    end

    %% --- ДОРОЖКА: HR ОТДЕЛ ---
    subgraph HR_Lane [HR-отдел]
        direction TB
        HR_CheckReq{Заявка<br>корректна?}:::decision
        HR_Screening[Проверка анкет]:::process
        HR_CheckCV{Кандидат<br>подходит?}:::decision
        HR_Interview[Первичное интервью]:::process
        HR_OfferDecision{Оба этапа<br>успешны?}:::decision
        HR_SendOffer[Отправить оффер]:::process
        HR_NotifyIT[Уведомить IT-отдел]:::process
    end

    %%  ДОРОЖКА: СИСТЕМА 
    subgraph System_Lane [Система]
        direction TB
        Sys_NotifyFix[Уведомление о<br>доработке]:::process
        Sys_Publish[Публикация вакансии]:::process
        Sys_Reject[Отправка отказа]:::process
        Sys_AddEmployee[Добавление в БД<br>сотрудников]:::process
    end

    %%  ДОРОЖКА: КАНДИДАТ 
    subgraph Candidate_Lane [Кандидат]
        direction TB
        Cand_Apply[Подача заявки]:::process
        Cand_AcceptOffer{Принять<br>оффер?}:::decision
    end

    %%  ДОРОЖКА: IT ОТДЕЛ 
    subgraph IT_Lane [IT-отдел]
        direction TB
        IT_Setup[Настройка рабочего места]:::process
    end






    %%  ЛОГИКА И СВЯЗИ 

    %% 1. Подготовительный этап
    M_CreateReq --> HR_CheckReq
    HR_CheckReq -- Нет --> Sys_NotifyFix
    Sys_NotifyFix --> M_FixReq
    M_FixReq --> HR_CheckReq
    HR_CheckReq -- Да --> Sys_Publish

    %% 2. Отбор (Публикация и заявки)
    Sys_Publish --> Cand_Apply
    Cand_Apply --> HR_Screening
    HR_Screening --> HR_CheckCV
    
    HR_CheckCV -- Нет --> Sys_Reject
    HR_CheckCV -- Да --> HR_Interview

    %% 3. Собеседования
    HR_Interview --> M_TechInterview
    M_TechInterview --> HR_OfferDecision
    
    HR_OfferDecision -- Нет (Провал) --> Sys_Reject
    
    %% 4. Оффер и Заключение
    HR_OfferDecision -- Да --> HR_SendOffer
    HR_SendOffer --> Cand_AcceptOffer
    
    Cand_AcceptOffer -- Нет --> Sys_Reject





    %% ВЕТВЛЕНИЕ (ПАРАЛЛЕЛЬНЫЕ ДЕЙСТВИЯ)
    Cand_AcceptOffer -- Да --> Fork_Final(( )) 
    style Fork_Final fill:#000,stroke:#000,width:40px

    Fork_Final --> Sys_AddEmployee
    Fork_Final --> HR_NotifyIT
    HR_NotifyIT --> IT_Setup





    %% ЗАВЕРШЕНИЕ
    Sys_AddEmployee --> Join_Final(( ))
    IT_Setup --> Join_Final
    style Join_Final fill:#000,stroke:#000,width:40px

    Join_Final --> End((Конец)):::startend
    Sys_Reject --> End



 ```
