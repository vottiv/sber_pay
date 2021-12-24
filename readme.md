# Подключение к платежной системе Сбербанка для Битрикса на чистом АПИ

## Вводные

Реализация под Битрикс была сделана на основе информации из [этой статьи](https://snipp.ru/php/sberbank-pay).

Функционал потребовался для создании ссылки для оплаты в мобильном приложении. Мобильное приложение обращается к сайту на платформе Битрикса.

## Внедрение функционала

1) Получим ссылку на оплату заказа, для этого вызовем
```php
$response = (new PaySystem\SberPay())->registerOrder($orderId);
```
2) В ответе будет formUrl, это поле с ссылкой на оплату в системе Сбера, она то нам и понадобится
3) Перейдём по ссылке, оплатим (есть [тестовые карты](https://securepayments.sberbank.ru/wiki/doku.php/test_cards)), произойдет редирект обратно на сайт - по той ссылке, которую указали в методе getReturnUrl().
4) На странице, куда нас перекинуло после оплаты, необходимо добавить проверку статуса оплаты.
```php
PaySystem\SberPay::getOrderStatus($orderId);
```
## Важно 

При регистрации заказа используется идентификатор заказа на сайте, а при проверке необходим идентификатор заказа из системы Сбера. 

Как вариант, можно создать новый HL-блок, который будет хранить оба идентификатора и на странице с проверкой статуса будет удаление этой записи, в случае получения успешной оплаты со стороны Сбера.

Рекомендую к прочтению [документацию от Сбера](https://developer.sberbank.ru/doc/v1/acquiring/rest-requests1pay).
