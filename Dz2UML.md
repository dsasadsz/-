```mermaid
graph LR
    %% States and Start Node
    Start(([*] Начало))
    Idle(1. Idle / Ожидание)
    WaitM(2. WaitingForMoney / Ждет денег)
    MoneyR(3. MoneyReceived / Деньги внесены)
    TicketD(4. TicketDispensed / Билет выдан)
    Canceled(5. TransactionCanceled / Отменено)

    %% Flow: Start -> Idle
    Start --> Idle

    %% Основной поток
    Idle -- SelectTicket / requestPayment() --> WaitM
    WaitM -- InsertMoney [amount >= price] / lockTicket() --> MoneyR
    MoneyR -- DispenseTicket / issueTicketAndChange() --> TicketD

    %% Сценарий отмены
    WaitM -- CancelTransaction / refundMoney() --> Canceled
    MoneyR -- CancelTransaction / refundMoney() --> Canceled
    
    %% Циклы (Возврат в исходное)
    TicketD --> Idle
    Canceled --> Idle
```






Акторы и События (Триггеры)
Событие 1: SelectTicket (Выбор билета)

Событие 2: InsertMoney (Внесение денег)

Событие 3: DispenseTicket (Выдача билета)

Событие 4: CancelTransaction (Отмена транзакции)
