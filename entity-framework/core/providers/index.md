---
title: "Поставщики базы данных — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Поставщики баз данных

Entity Framework Core использует модель поставщика, чтобы позволить использовать EF для доступа к множеству разных баз данных. Некоторые концепции являются общими для большинства баз данных и включены в основной набор компонентов EF Core. К ним относится выражение запросов с помощью LINQ, транзакции и отслеживание изменений объектов при их загрузке из базы данных. Некоторые концепции характерны для определенного поставщика. Например, поставщик SQL Server позволяет настроить таблицы, оптимизированные для памяти (функция, относящаяся к SQL Server). Другие концепции характерны для класса поставщиков. Например, поставщики EF Core для реляционных баз данных основаны на общей библиотеке `Microsoft.EntityFrameworkCore.Relational`, которая предоставляет API для настройки сопоставлений столбцов и таблиц, ограничения внешнего ключа и т. п.

Поставщики EF Core поступают из самых разных источников. Не все поставщики разрабатываются в рамках проекта Entity Framework Core. Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.