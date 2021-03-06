# Типовая обработка для 1С:УПП
Позволяет:

- [Настроить соответствие номенклатуры и цветов, между вашей базой 1С и _Производителем_](https://github.com/oknosoft/windowbuilder-parametric/blob/master/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D0%B8%D1%8F%20%D0%BF%D0%BE%20%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B5%20%D1%81%D0%BE%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B8%D0%B9.md);
- [Выполнить стартовую автоматическую настройку соответствий](https://github.com/oknosoft/windowbuilder-parametric/blob/master/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D0%B8%D1%8F%20%D0%BF%D0%BE%20%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B5%20%D1%81%D0%BE%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B8%D0%B9.md);
- Выгрузить заказ поставщику в сервис, получить обратно цены и установить их в заказе поставщику;
- Прочитать и посмотреть справочники номенклатуры и цветов _Производителя_;
- Прочитать и посмотреть состояние заказа в сервисе;
- Проанализировать логи сервиса.

### Как подключить обработку для отправки заказов
Первое, что нужно сделать - указать параметры авторизации.
Они находятся в макете Подключение, до которого нужно добраться через конфигуратор. Менять нужно значения параметров login, password, suffix.
Если есть трудности с этим, сообщите нам, мы вышлем вам обработку с уже указанными вашими параметрами подключения.

Все функции обработки доступны и без какого-либо внедрения в конфигурацию - достаточно открыть обработку как внешнюю, через меню Файл - Открыть. Можно выгружать заказы, настраивать соответствия, смотреть справочники и логи, и т.д.

Но понятно, что людям выгружать заказы в сервис удобнее, нажимая кнопку в заказе поставщику (или в списке заказов).

Вызов обработки для выгрузки заказа поставщику в сервис очень прост:

- нужно создать ОбработкуОбъект (в зависимости от способа интеграции, например Обработки.ПараметрическийЗаказ2.Создать(), если обработка находится в составе конфигурации);
- вызвать функцию ОбработкаОбъект.Записать(ЗаказОбъект), где ЗаказОбъект - это заказ поставщику, документ объект (не ссылка).

Вариантов, как включить обработку в состав решения, много. Все программисты 1С знают массу вариантов, поэтому не будем долго расписывать.
Самый простой - включить обработку в состав конфигурации, тогда код вызова из заказа поставщику будет очень простым.
Можно включить во внешние обработки, но тогда вызвать будет чуть сложнее.

### Выгрузка заказа в сервис, получение цен
Если обработка открывается как файл, то достаточно указать в ней заказ, нажать кнопку "Записать заказ в сервис".
Если обработка подключена к заказу поставщику, то нужно выполнить действия, которые придумали программисты (это может быть кнопка, какой-нибудь АРМ и т.д.).

Что происходит при выгрузке в сервис:
- заказ отправится в сервис;
- вернется ответ, что заказ записался и рассчитался;
- вернутся цены и суммы, которые сравнятся с текущим заказом поставщику;
- если цены отличаются, будет выдано сообщение;
- если обработка открыта как файл, то при изменении цен будет открыта форма заказа поставщику, и новые цены туда подставятся;
- если обработка открыта из заказа поставщику, то при изменении цен они сразу подставятся в форму заказа.

### Просмотр справочников _Производителя_
Чтобы посмотреть справочники номенклатуры и цветов, нужно нажать кнопку "Еще" - "Посмотреть справочники из сервиса".

### Просмотр состояния заказа в сервисе
Чтобы посмотреть состояние заказа в сервисе, нужно указать заказ и нажать кнопку "Прочитать заказ из сервиса".

### Анализ логов микросервиса
С помощью [обработки](https://github.com/oknosoft/windowbuilder-parametric/blob/master/1c/%D0%9F%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9%D0%97%D0%B0%D0%BA%D0%B0%D0%B72.epf) можно анализировать логи микросервиса для своей зоны.
Нужно скачать обработку, в макете Подключение указать свои учетные данные. Открыть обработку, нажать "Еще" - "Анализ логов".
Выбрать период, нажать "Прочитать". Флажок "Включая запросы логов" включает/выключает добавление в результат запросы на предоставление логов.

Анализ логов дает следующую информацию:

- дата запроса;
- наличие ошибки и сообщение об ошибке (error и message);
- информацию об отсутствии лога за конкретную дату (если запросов в этот день не было);
- время запроса (Start);
- url запроса (url);
- метод запроса (method, get/post);
- ip-адрес, с которого пришел запрос (ip);
- заголовки запроса (headers);
- тело (данные запроса, post_data);
- ответ на запрос (response);
- время выполнения запроса (duration).
