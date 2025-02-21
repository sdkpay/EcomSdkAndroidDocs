# EcomSdkAndroidDocs

##### [Начало работы](https://sdkpay.github.io/EcomSdkAndroidDocs/start) | [Сценарий оплаты](https://sdkpay.github.io/EcomSdkAndroidDocs/payment_script) | [Работа в режиме песочницы](https://sdkpay.github.io/EcomSdkAndroidDocs/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/EcomSdkAndroidDocs/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/EcomSdkAndroidDocs/version)
---
# Вспомогательные структуры данных

## EcomSdkSetupConfig
#### Конфиг для инициализации SDK

|Параметр|Тип|Обязательный|Описание|
|---|---|---|---|
|context|Context|Да|Context или ApplicationContext приложения|
|stage|EcomSdkStage|Да|Список стендов для работы с EcomSdk.<br>Структура [EcomSdkStage](https://sdkpay.github.io/EcomSdkAndroidDocs/data_structures#ecomsdkstage)|
|disabledFeatures|List\<EcomSdkFeature\>|Нет|Список выключенных features. Дефолтное значение listOf().<br>Структура [EcomSdkFeature](https://sdkpay.github.io/EcomSdkAndroidDocs/data_structures#ecomsdkfeature)|
|enableLoggingByMerchant|Boolean|Нет|Флаг включенного логирования для партнера. Дефолтное значение true|
|callback|(Boolean) -> Unit|Да|Блок, отрабатыващий после настройки SDK. Корректное значение колбэка true|

## EcomSdkStage
#### Стенды SDK
```
/**
 * Enum со стендами SDK
 * */
public enum class EcomSdkStage: Stage {
    /** Продовый стенд */
    Prod,
    /** Песочница */
    SandBox
}
```

## EcomSdkFeature
#### Фичи SDK
```
/**
 * Enum с фичами SDK
 * */
enum class EcomSdkFeature {
    /** Отображение результирующих экранов */
    RESULT_VIEW,
    /** Приём платежей через СБП */
    PAY_BY_SBP,
    /** Приём платежей через SPaySDK */
    PAY_BY_SPAY,
}
```

## EcomSdkMerchantOptionsConfig
#### Конфиг для запуска сценария оплаты методом *pay()*

|Параметр|Тип|Обязательный|Описание|
|---|---|---|---|
|context|Context|Да|ActivityContext приложения|
|bankInvoiceId|String|Да|Уникальный идентификатор заказа в Платежном шлюзе Банка. Необходимо передавать значение sbolBankInvoiceId из ответа на Запрос регистрации заказа|
|apiKey|String|Да|Ключ для работы с сервисами платежного шлюза через SDK|
|merchantLogin|String|Да|Логин для работы с сервисами платежного шлюза|
|orderNumber|String|Да|Уникальный идентификатор заказа в системе Партнера|
|appPackageName|String|Да|Package (BuildConfig.APPLICATION_ID) приложения, по которому необходимо вернуть Плательщика в приложение Партнера, после аутентификации в СберБанк Онлайн|
|callback|(EcomSdkResult) -> Unit|Да|Блок, отрабатыващий после завершения сценария оплаты Плательщиком, возвращающий результат оплаты.<br>Структура [EcomSdkResult](https://sdkpay.github.io/EcomSdkAndroidDocs/data_structures#ecomsdkresult)|

## EcomSdkResult
#### Результат выполнения метода *pay()*

```
/**
* Возможные результаты проведения оплаты
*
* @property Success оплата произведена успешно
* @property Waiting статус оплаты неизвестен
* @property Cancel оплата отменена пользователем
* @property Error оплата не была выполнена из-за ошибки
*/
sealed interface EcomSdkResult {
    data object Success : EcomSdkResult

    data object Waiting : EcomSdkResult

    data object Cancel : EcomSdkResult

    /**
    * Класс ошибки при работе с EcomSdk
    *
    * @param httpCode код ответа http
    * @param errorCode внутренний код ответа от севера
    * @param description описание ошибки
    */
    data class Error(
        val httpCode: String? = null,
        val errorCode: String? = null,
        val description: String? = null
    ) : EcomSdkResult
} 
```

## Платежные инструменты для работы в режиме песочницы

| Тип тестирования | Номер карты | Срок действия карты | CVV |  SMS-код | Пароль |
| --- | --- | --- | --- | --- | --- |
| Без 3ds | 4279380620378929 | 06/26 | 353 | нет | нет |
| С 3ds | 2202208020207685 | 05/27 | 133 | 111111 | нет |
| С 3ds | 2201382000000047 | 05/27 | 133 | нет | 1qwezxc |

