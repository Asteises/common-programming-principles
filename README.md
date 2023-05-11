# Основные принципы программирования

Каждый может написать код... А что на счет хорошего кода? Просто запомните данные принципы, и ваш код всегда будет мягким и шелковистым =)

## KISS

"Keep it short and simple" - делай коротко и просто.

+ Делай максимально простую и понятную архитектуру. 
+ Применяй шаблоны проектирования.
+ Не изобретай велосипед.
+ Не усложняй! 

## DRY

"Don't repeat yourself" - не повторяйся.

Хотя достичь 100% исполнения принципа DRY в проектах практически не реально, стремиться к этому нужно.
Не очертя голову, конечно. Но если поймали себя на мысли, что этот код вы уже писали, то остановитесь и подумайте, - "Можно ли избежать дублирования кода?".

## YAGNI

"You aren't gonna need it" - тебе это не понадобится.

Пишите только тот код, который нужен здесь и сейчас. Удаляйте неиспользуемые классы, методы, комментариии. Все то, что отвлекает от текущей задачи.


## BDUF

"Big design up front" - проектирование прежде всего.

Продумайте архитектуру и процессы приложения прежде чем переходить к реализации.
Стоимость планирования всегда меньше стоимости разработки.

## SOLID

Самый главный из принципов программирования, состоит из нескольких:

### S - Single Responsibility Principle - принцип единственной ответственности

Каждый класс должен иметь только одну зону ответственности.

Например, мы пишем приложение для аренды велосипедов, и у нас есть класс RentBikeService с несколькими методами: 
+ найти велосипед по номеру
+ забронировать велосипед
+ распечатать бланк заказа
+ получить информацию о велосипеде
+ отправить сообщение

Вот как это может выглядеть:

```
public class RentBikeService {

    public Bike findBike(String bikeNumber) {
    
        //find bike by number
        return bike;
    }

    public Order orderBike(String bikeNumber, Client client) {
    
        //client order bike
        return order;
    }

    public void printOrder(Order order) {
    
        //print order
    }
    
    public void getBikeInfo(String bikeType) {
        if (bikeType.equals("classic")) {
        
            //do something
        }
        if (bikeType.equals("mountin")) {
        
            //do something
        }
        if (bikeType.equals("child")) {
        
            //do something
        }
    }
    
    public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
        
            //write email
            //use email API
        }
    }
}
```

Казалось бы, все методы уместны, каждый занимается своей задачей, но сам класс... Нарушает принцип единственной ответственности, он отвечает за все и сразу.
Нужно разделить данный класс на несколько, предоставив каждому только одну зону ответственности.


Класс, отвечающий за выбор велосипеда:
```
public class BikeService {

    public Bike findBike(String bikeNumber) {
    
        //find bike by number
        return bike;
    }
}
```


Класс, отвечающий за печать бланка с заказом:
```
public class PrinterService {

    public void printOrder(Order order) {
        //print order
    }
}
```


Класс, отвечающий за информацию о каждом велосипеде:
```
public class BikeInfoService {

    public void getBikeInfo(String bikeType) {
        if (bikeType.equals("classic")) {
        
            //do something
        }
        if (bikeType.equals("mountin")) {
        
            //do something
        }
        if (bikeType.equals("child")) {
        
            //do something
        }
    }
}
```


Класс, отвечающий за отправку сообщений:
```
public class NotificationService {

  public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
        
            //write email
            //use email API
        }
    }
}
```

Теперь в классе RentBikeService остался только один метод:
```
public class RentBikeService {

    public Order orderBike(String bikeNumber, Client client) {
    
        //client order bike
        return order;
    }
```

Сейчас в нашем приложении каждый класс имеет только одну зону ответственности, а следовательно, только одну причину для изменения.


### O - Open closed Principle - принцип открытости-закрытости

Классы должны быть открыты для расширения, но закрыты для изменения.

К примеру, если мы захотим добавить новый способ отправки сообщений пользователям (через SMS), то нужно будет внести изменение в класс NotificationService:

```
public class NotificationService {

  public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
        
            //write email
            //use email API
        }
        if (typeMessage.equals("sms")) {
        
            //write sms
            //use sms API
        }
    }
}
```

В данном случае мы нарушим второй принцип. Так как изменяем класс, а должны только раасширять. 

Нужно спроектировать так, чтобы использовать функцию отправки сообщений повторно, просто расширив ее функционал. Для этого идеально подойдет интерфейс:

```
public interface NotificationService {
    public void sendMessage(String message);
}
```

Теперь, если нам понадобиться отправлять сообщения через электронную почту, то мы создадим новый класс и имплементируем интерфейс передачи сообщений:

```
public class EmailNotification implements NotificationService {

    @Overrride
    public void sendMessage(String message) {
    
        //write email
        //use email API
    }
}
```

То же самое сделаем, если клиент поставит задачу отправлять сообщения через СМС:

```
public class SmsNotification implements NotificationService {

    @Overrride
    public void sendMessage(String message) {
    
        //write sms
        //use sms API
    }
}
```

Таким образом, мы не модифицируем (изменяем) наш класс, а расширяем его функциональность. 

### L - Liskov substitution Principle - принцип подстановки Барбары Лисков

Должна быть возможность вместо базового (родительского) типа (класса) подставить любой его подтип (класс-наследник), при этом работа программы не должна измениться.

Данный принцип тесно связан с наследованием классов.

Представим, что у нас есть базовый класс, отвечающий за счет клиента в приложении. В нем есть три метода:
+ просмотр остатка на счете
+ пополнение счета
+ оплата услуг.

Так это может выглядеть:

```
public class Account {

    public BigDecimal balance(String numberAccount){
    
        //some logic
        return bigDecimal;
    };
    
    public void refill(String numberAccount, BigDecimal sum){
    
        //some logic
    }
    public void payment(String numberAccount, BigDecimal sum){
    
        //some logic
    }
}
```

Далее, нам необходимо написать два новых класса, отвечающих за зарплатный счет клиента и депозитный счет. Причем, зарплатный счет будет поддерживать все методы базового класса, а депозитный счет не должен поддерживать метод проведения оплаты:

```
public class SalaryAccount extends Account{

    @Override
    public BigDecimal balance(String numberAccount){
        //some logic
        return bigDecimal;
    };
    
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //some logic
    }
    
    @Override
    public void payment(String numberAccount, BigDecimal sum){
        //some logic
    }
}
```

```
public class DepositAccount extends Account{

    @Override
    public BigDecimal balance(String numberAccount){
        //some logic
        return bigDecimal;
    };
    
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //some logic
    }
    
    @Override
    public void payment(String numberAccount, BigDecimal sum){
        throw new UnsupportedOperationException("Operation not supported");
    }
}
```

Теперь, если мы захотим заменить в приложении класс Accaunt на класс-наследник SalaryAccount, то программа продолжит работать корректно, так как в классе SalaryAccount доступны все методы родительского класса.

Но если мы попробуем сделать тоже самое с классом DepositAccount, то программа будет выбрасывать исключение при вызове метода payment(), а этого быть не должно. Произойдет нарушение третьего приниципа.

Чтобы этого избежать, необходимо в базовый класс выносить только общую логику, характерную для всех классов наследников. В нашем случае стоит сделать так:

Изменяем класс Account:

```
public class Account {

    public BigDecimal balance(String numberAccount){
        //some logic
        return bigDecimal;
    };
    
    public void refill(String numberAccount, BigDecimal sum){
        //some logic
    }
}
```

Наследум от него DepositAccount:
```
public class DepositAccount extends Account{

    @Override
    public BigDecimal balance(String numberAccount){
        //some logic
        return bigDecimal;
    };
    
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //some logic
    }
}
```

Создадим дополнительный класс PaymentAccount, который унаследуем от Account и его расширим методом проведения оплаты.

```
public class PaymentAccount extends Account{
    public void payment(String numberAccount, BigDecimal sum){
        //some logic
    }
}
```

И наш класс SalaryAccount уже унаследуем от класса PaymentAccount.

```
public class SalaryAccount extends PaymentAccount{

    @Override
    public BigDecimal balance(String numberAccount){
        //some logic
        return bigDecimal;
    };
    
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //some logic
    }
    
    @Override
    public void payment(String numberAccount, BigDecimal sum){
        //some logic
    }
}
```

Сейчас замена класса PaymentAccount на его класс-наследник SalaryAccount не "поломает" нашу программу, так как класс SalaryAccount имеет доступ ко всем методам, что и PaymentAccount. Также все будет хорошо при замене класса Account на его класс-наследник PaymentAccount.


Принцип подстановки Барбары Лисков заключается в правильном использовании отношения наследования. Мы должны создавать наследников какого-либо базового класса тогда и только тогда, когда они собираются правильно реализовать его логику, не вызывая проблем при замене родителей на наследников.
### I -  Interface Segregation Principle - принцип разделения интерфейсов

Данный принцип обозначает, что не нужно заставлять клиента (класс) реализовывать интерфейс, который не имеет к нему отношения.


Допустим у нас имеется интерфейс Payments и в нем есть три метода: 
+ оплата WebMoney
+ оплата банковской карточкой
+ оплата по номеру телефона.

```
public interface Payments {

    void payWebMoney();
    void payCreditCard();
    void payPhoneNumber();
}
```

Далее нам надо реализовать два класса-сервиса, которые будут у себя реализовывать различные виды проведения оплат (класс InternetPaymentService и TerminalPaymentService). При этом TerminalPaymentService не будет поддерживать проведение оплат по номеру телефона. Но если мы оба класса имплементим от интерфейса Payments, то мы будем "заставлять" TerminalPaymentService реализовывать метод, который ему не нужен.

```
public class InternetPaymentService implements Payments{

    @Override
    public void payWebMoney() {
        //logic
    }
    
    @Override
    public void payCreditCard() {
        //logic
    }
    
    @Override
    public void payPhoneNumber() {
        //logic
    }
}
```

```
public class TerminalPaymentService implements Payments{

    @Override
    public void payWebMoney() {
        //logic
    }
    
    @Override
    public void payCreditCard() {
        //logic
    }
    
    @Override
    public void payPhoneNumber() {
        //???????
    }
}
```

Таким образом произойдет нарушение принципа разделения интерфейсов.


Для того чтобы этого не происходило необходимо разделить наш исходный интерфейс Payments на несколько и, создавая классы, имплементить в них только те интерфейсы с методами, которые им нужны:

```
public interface WebMoneyPayment {
    void payWebMoney();
}
```

```
public interface CreditCardPayment {
    void payCreditCard();
}
```

```
public interface PhoneNumberPayment {
    void payPhoneNumber();
}
```

```
public class InternetPaymentService implements WebMoneyPayment,
                                                CreditCardPayment, 
                                                  PhoneNumberPayment{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
    @Override
    public void payPhoneNumber() {
        //logic
    }
}
```

```
public class TerminalPaymentService implements WebMoneyPayment, CreditCardPayment{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
}
```

### D - Dependency Inversion Principle - принцип инверсии зависимостей

Модули верхнего уровня не должны зависеть от модулей нижнего уровня. И те, и другие должны зависеть от абстракции. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.


Допустим мы пишем приложение для магазина и решаем вопросы с проведением оплат. Вначале это просто небольшой магазин, где оплата происходит только за наличные. Создаем класс Cash и класс Shop.

```
public class Cash {
    public void doTransaction(BigDecimal amount){
        //logic
    }
}
```

```
public class Shop {
    private Cash cash;
    public Shop(Cash cash) {
        this.cash = cash;
    }
    public void doPayment(Object order, BigDecimal amount){
        cash.doTransaction(amount);
    }
}
```

Вроде все хорошо, но мы уже нарушили принцип инверсии зависимостей, так как мы тесно связали оплату наличными к нашему магазину. И если в дальнейшем нам необходимо будет добавить оплату еще банковской картой и телефоном ("100% понадобится"), то нам придется переписывать и изменять много кода. Мы в нашем коде модуль верхнего уровня тесно связали с модулем нижнего уровня, а нужно чтобы оба уровня зависели от абстракции.


Поэтому создадим интерфейс Payments:

```
public interface Payments {
    void doTransaction(BigDecimal amount);
}
```

Теперь все наши классы по оплате будут имплементить данный интерфейс:

```
public class Cash implements Payments{
    @Override
    public void doTransaction(BigDecimal amount) {
        //logic
    }
}
```

```
public class BankCard implements Payments{
    @Override
    public void doTransaction(BigDecimal amount) {
         //logic
    }
}
```

```
public class PayByPhone implements Payments {
    @Override
    public void doTransaction(BigDecimal amount) {
        //logic 
    }
}
```

Теперь надо перепроектировать реализацию нашего магазина:

```
public class Shop {
    private Payments payments;

    public Shop(Payments payments) {
        this.payments = payments;
    }

    public void doPayment(Object order, BigDecimal amount){
        payments.doTransaction(amount);
    }
}
```

Сейчас наш магазин слабо связан с системой оплаты, то есть он зависит от абстракции и уже не важно каким способом оплаты будут пользоваться (наличными, картой или телефоном) все будет работать.
