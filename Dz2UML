```mermaid

stateDiagram-v2
    direction LR
    
    [*] --> Idle: [Начало работы]
    
    state Idle {
        Idle : Ожидание действия
        state Idle_Nested
    }
    
    state WaitingForMoney {
        WaitingForMoney : Ждет внесения суммы
        state WaitingForMoney_Nested
    }
    
    state MoneyReceived {
        MoneyReceived : Внесена достаточная сумма
        state MoneyReceived_Nested
    }
    
    state TicketDispensed {
        TicketDispensed : Билет выдан
        state TicketDispensed_Nested
    }
    
    state TransactionCanceled {
        TransactionCanceled : Сброс/Возврат денег
        state TransactionCanceled_Nested
    }

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
