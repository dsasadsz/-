```mermaid
stateDiagram-v2
    direction LR
    
    %% Начальное состояние
    [*] --> Idle: Начать работу

    %% Описание состояний (имя : описание)
    Idle : Ожидание (Idle)
    WaitingForMoney : Ожидание денег
    MoneyReceived : Деньги получены
    TicketDispensed : Билет выдан
    TransactionCanceled : Отмена операции

    %% ПЕРЕХОДЫ
    
    %% 1. Выбор билета
    Idle --> WaitingForMoney: Выбор билета
    
    %% 2. Внесение денег
    WaitingForMoney --> MoneyReceived: Внесена полная сумма
    
    %% 3. Выдача билета
    MoneyReceived --> TicketDispensed: Выдать билет
    
    %% 4. Возврат в начало после выдачи
    TicketDispensed --> Idle: Забрать билет

    %% 5. Сценарии отмены
    WaitingForMoney --> TransactionCanceled: Отмена клиентом
    MoneyReceived --> TransactionCanceled: Отмена клиентом
    
    %% 6. Возврат после отмены
    TransactionCanceled --> Idle: Возврат средств
```













Акторы и События (Триггеры)
Событие 1: SelectTicket (Выбор билета)

Событие 2: InsertMoney (Внесение денег)

Событие 3: DispenseTicket (Выдача билета)

Событие 4: CancelTransaction (Отмена транзакции)
