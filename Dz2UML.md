```mermaid

stateDiagram-v2
    direction LR
    
    [*] --> Idle
    
    state Idle
    state WaitingForMoney
    state MoneyReceived
    state TicketDispensed
    state TransactionCanceled

    %% Описание состояний:
    note right of Idle: Ожидание действия
    note right of WaitingForMoney: Ждет внесения суммы
    note right of MoneyReceived: Внесена достаточная сумма
    note right of TicketDispensed: Билет выдан
    note right of TransactionCanceled: Сброс/Возврат денег

    %% Основной поток
    Idle --> WaitingForMoney: SelectTicket / requestPayment()
    WaitingForMoney --> MoneyReceived: InsertMoney [amount >= price] / lockTicket()
    MoneyReceived --> TicketDispensed: DispenseTicket / issueTicketAndChange()
    TicketDispensed --> Idle: [Возврат в исходное]

    %% Сценарий отмены
    WaitingForMoney --> TransactionCanceled: CancelTransaction / refundMoney()
    MoneyReceived --> TransactionCanceled: CancelTransaction / refundMoney()
    
    %% Завершение транзакции
    TransactionCanceled --> Idle: [Возврат в исходное]
```






Акторы и События (Триггеры)
Событие 1: SelectTicket (Выбор билета)

Событие 2: InsertMoney (Внесение денег)

Событие 3: DispenseTicket (Выдача билета)

Событие 4: CancelTransaction (Отмена транзакции)
