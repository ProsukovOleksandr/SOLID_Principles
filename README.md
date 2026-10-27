№1 
Принцип єдиної відповідальності стверджує, що клас повинен мати лише одну причину для зміни. Це означає, що кожен клас має відповідати за одну окрему частину функціональності програми. Наприклад, якщо клас одночасно перевіряє дані користувача, зберігає їх у базі даних і надсилає електронні листи, він порушує SRP. Зміни в будь-якій із цих функцій вимагатимуть редагування одного й того самого класу. Щоб дотримуватися цього принципу, потрібно розділяти відповідальності між окремими класами. Це спрощує тестування, підтримку та розширення програми, а також зменшує ризик виникнення помилок під час змін.
Приклад коду
class UserValidator:
    def validate(self, username, email):
        if not username or not email:
            raise ValueError("Некоректні дані")


class UserRepository:
    def save(self, username, email):
        with open("users.txt", "a") as file:
            file.write(f"{username},{email}\n")


class EmailService:
    def send_welcome_email(self, email):
        print(f"Лист надіслано користувачу {email}")


class UserManager:
    def __init__(self):
        self.validator = UserValidator()
        self.repository = UserRepository()
        self.email_service = EmailService()

    def register_user(self, username, email):
        self.validator.validate(username, email)
        self.repository.save(username, email)
        self.email_service.send_welcome_email(email)

№2
Принцип відкритості/закритості (Open/Closed Principle, OCP) стверджує, що програмні сутності (класи, модулі, функції) повинні бути відкритими для розширення, але закритими для модифікації. Це означає, що нову функціональність потрібно додавати без необхідності змінювати вже написаний і перевірений код. Для цього можна використовувати абстракції, інтерфейси, успадкування та поліморфізм.Дотримання OCP спрощує розширення програми, зменшує ризик появи помилок і полегшує супровід програмного забезпечення.
Приклад коду
from abc import ABC, abstractmethod


class PaymentMethod(ABC):
    @abstractmethod
    def pay(self, amount):
        pass


class CardPayment(PaymentMethod):
    def pay(self, amount):
        print(f"Оплата карткою: {amount} грн")


class PayPalPayment(PaymentMethod):
    def pay(self, amount):
        print(f"Оплата через PayPal: {amount} грн")


class CryptoPayment(PaymentMethod):
    def pay(self, amount):
        print(f"Оплата криптовалютою: {amount} грн")


class PaymentProcessor:
    def process(self, payment_method, amount):
        payment_method.pay(amount)


processor = PaymentProcessor()

processor.process(CardPayment(), 500)
processor.process(PayPalPayment(), 700)
processor.process(CryptoPayment(), 1000)


№3
Принцип підстановки Лісков (Liskov Substitution Principle, LSP) стверджує, що об’єкти дочірніх класів повинні бути взаємозамінними з об’єктами батьківського класу без порушення правильності роботи програми. Це означає, що дочірній клас повинен зберігати очікувану поведінку батьківського класу та виконувати його контракт. Якщо заміна одного об’єкта іншим призводить до помилок або несподіваної поведінки, принцип LSP порушується. Дотримання цього принципу робить програму більш гнучкою, надійною та полегшує розширення функціональності.
Приклад коду
class Bird:
    def eat(self):
        print("Птах їсть")


class FlyingBird(Bird):
    def fly(self):
        print("Птах летить")


class Sparrow(FlyingBird):
    def fly(self):
        print("Горобець летить")


class Penguin(Bird):
    def swim(self):
        print("Пінгвін плаває")


def feed_bird(bird):
    bird.eat()


def make_bird_fly(bird):
    bird.fly()


feed_bird(Sparrow())       # Працює правильно
feed_bird(Penguin())       # Працює правильно
make_bird_fly(Sparrow())   # Працює правильно


№4
Принцип розділення інтерфейсу (Interface Segregation Principle, ISP) стверджує, що клієнти не повинні залежати від методів, які вони не використовують. Це означає, що замість одного великого інтерфейсу, який містить багато різних методів, краще створювати кілька менших і спеціалізованих інтерфейсів. Кожен клас повинен реалізовувати лише ті операції, які йому дійсно потрібні. Дотримання ISP зменшує залежність між компонентами, спрощує підтримку коду та полегшує додавання нової функціональності.
Приклад коду
from abc import ABC, abstractmethod


class Workable(ABC):
    @abstractmethod
    def work(self):
        pass


class Eatable(ABC):
    @abstractmethod
    def eat(self):
        pass


class HumanWorker(Workable, Eatable):
    def work(self):
        print("Людина працює")

    def eat(self):
        print("Людина їсть")


class RobotWorker(Workable):
    def work(self):
        print("Робот працює")


human = HumanWorker()
robot = RobotWorker()

human.work()
human.eat()

robot.work()


№5
Принцип інверсії залежностей (Dependency Inversion Principle, DIP) стверджує, що модулі високого рівня не повинні залежати від модулів низького рівня. Обидва типи модулів повинні залежати від абстракцій. Також абстракції не повинні залежати від деталей реалізації, навпаки — деталі повинні залежати від абстракцій. Це означає, що бізнес-логіка програми не повинна бути жорстко прив’язана до конкретної бази даних, поштового сервісу чи іншого технічного компонента. Замість цього потрібно використовувати інтерфейси або абстрактні класи. Дотримання DIP робить програму гнучкішою, спрощує тестування та дозволяє замінювати окремі компоненти без значних змін основного коду.
Приклад коду
from abc import ABC, abstractmethod


class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass


class MySQLDatabase(Database):
    def save(self, data):
        print(f"Збереження в MySQL: {data}")


class PostgreSQLDatabase(Database):
    def save(self, data):
        print(f"Збереження в PostgreSQL: {data}")


class UserService:
    def __init__(self, database: Database):
        self.database = database

    def save_user(self, username):
        self.database.save(username)


mysql = MySQLDatabase()
service = UserService(mysql)
service.save_user("Іван")

postgres = PostgreSQLDatabase()
service2 = UserService(postgres)
service2.save_user("Олена")