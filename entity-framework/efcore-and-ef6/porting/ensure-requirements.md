---
title: "Перенос приложений из EF6 в основные EF – Проверка требований"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Перед развертыванием с EF6 EF ядра: проверка требования вашего приложения

Перед началом процедуры перевода важно проверить соответствие EF основных требований доступа к данным для вашего приложения.

## <a name="missing-features"></a>Для отсутствующих компонентов

Убедитесь, что EF Core все компоненты, необходимые для использования в приложении. В разделе [сравнение функций](../features.md) для выполнения в ядре EF набор функций, в сравнении с EF6 подробное сравнение. Если все необходимые компоненты отсутствуют, убедитесь, вы можете компенсировать отсутствие этих функций перед развертыванием EF ядра.

## <a name="behavior-changes"></a>Изменения в поведении

Это неполном списке внесены изменения в поведении между EF6 и EF Core. Важно сохранить эти учитывать как порт приложения, они могут изменять способ приложение ведет себя, но не будут отображаться как ошибки компиляции после замены EF ядра.

### <a name="dbsetaddattach-and-graph-behavior"></a>Поведение DbSet.Add/Attach и диаграммы

В EF6 вызвав `DbSet.Add()` в сущности приводит к рекурсивный поиск для всех сущностей, на которые ссылается его свойства навигации. Все сущности, которые находятся, а не уже отслеживается контекстом, которые также будут отмечены как добавлен. `DbSet.Attach()`ведет себя так же, за исключением того, все сущности, помечаются как без изменений.

**EF Core выполняет аналогичные рекурсивный поиск, но с некоторыми немного другие правила.**

*  Корневой объект всегда находится в запрошенное состояние (добавлен для `DbSet.Add` и без изменений для `DbSet.Attach`).

*  **Для сущностей, которые были обнаружены во время рекурсивный поиск свойств навигации:**

    *  **Если первичный ключ сущности, формируемый хранилищем**

        * Если первичный ключ не присвоено значение, переходит в состояние до добавлены. Значение первичного ключа считается «не установлено», если ей назначается значение по умолчанию для типа свойства CLR (т. е. `0` для `int`, `null` для `string`и т. д.).

        * Если первичный ключ присвоено значение, состояние имеет значение без изменений.

    *  Если первичный ключ не создается базой данных, сущность будет помещен в том же состоянии, как корень.

### <a name="code-first-database-initialization"></a>Код инициализации первой базы данных

**EF6 имеет значительный объем magic, выполняемых по выбору подключения к базе данных и инициализации базы данных. Ниже перечислены некоторые из этих правил.**

* Если конфигурация не выполняется, EF6 выберет базу данных на SQL Express или LocalDb.

* Если строка подключения с тем же именем, как контекст в приложениях `App/Web.config` файла, будет использоваться это соединение.

* Если базы данных не существует, он создается.

* Если ни одна из таблиц из модели существуют в базе данных, схемы для текущей модели добавляется в базу данных. Если миграция включена, они используются для создания базы данных.

* Если база данных существует и EF6 был создан ранее схемы, схемы проверяется на совместимость с текущей моделью. Если модель данных была изменена с момента создания схемы исключение.

**Основные EF не выполняет это магическое значение.**

* Подключение к базе данных должен быть явно настроен в коде.

* То инициализация не выполняется. Необходимо использовать `DbContext.Database.Migrate()` для применения миграций (или `DbContext.Database.EnsureCreated()` и `EnsureDeleted()` для создания и удаления базы данных без использования миграции).

### <a name="code-first-table-naming-convention"></a>Код соглашения об именах для первой таблицы

EF6 завершит службы преобразования во множественную форму для вычисления имя таблицы по умолчанию, который сопоставляется сущность с именем класса сущностей.

EF Core использует имя `DbSet` свойства, предоставляемую сущности в производном контекста. Если сущность не имеет `DbSet` используется свойство, а затем имени класса.
